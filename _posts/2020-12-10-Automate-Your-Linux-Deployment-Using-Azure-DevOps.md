---
title: Automate Your Linux Deployment Using Azure DevOps
description: >-
  A comprehensive guide on utilizing Azure DevOps to the fullest using CI/CD and
  shell scripts.
date: '2020-12-10T12:49:28.426Z'
layout: single
author: Agustinus Theodorus
categories: ['tech']
keywords: []
slug: >-
  /automate-your-linux-deployment-using-azure-devops
header:
  teaser: https://cdn-images-1.medium.com/max/800/0*zCSfGwYvrx7hc3DY
comments:
  provider: "disqus"
  disqus:
    shortname: "agustinustheo"
---

Feel like you have been logging into your Linux servers too much lately? Good, this might be the right article for you. In teams, either large or small manual deployments can be tedious. But they are also predictable. Deployment steps don’t really change that often unless you have a breaking change or you are implementing a major feature.

![](https://cdn-images-1.medium.com/max/800/0*zCSfGwYvrx7hc3DY)

> So, why don’t you just automate it? You don’t need to do it yourself. Do important things like **writing code** and let a machine do the deployments for you.

If your team is blessed to use Azure DevOps here is a tutorial to finally automate **_the hell_** out of your deployments. You _seldom_ have to log on to your Linux servers again.

**In this tutorial, we will be making an automated pipeline to deploy .NET Core applications to a Centos 8 server. The app will be run as a service and we will serve those services through a reverse proxy using NGINX.**

### Installing Azure Agent on Linux

In Azure DevOps, we use an agent to interact with our builds in the pipeline. Installing agents are straightforward, just follow these steps:

#### 1\. Setting Up A Deployment Group in Azure DevOps

Deployment groups are the list of agents already installed on your servers. You would need to make a deployment group for each server.

First, open your **Azure DevOps** dashboard.

![](https://cdn-images-1.medium.com/max/800/1*diKrTk5BKavbs076x6AqNQ.png)

Next, open your **Pipelines**

![](https://cdn-images-1.medium.com/max/800/0*RQiW01F5omxuvnAV)

Go to **Deployment Groups**, and click **New**.

![](https://cdn-images-1.medium.com/max/800/0*wFxLJNCYSxD9PhN4)

Give your Linux VM an Alias, and the description. Then click **Create**.

![](https://cdn-images-1.medium.com/max/800/1*dC9GLQyFwPz4sVprOJ4l-g.png)

You have created a deployment group, now to install this deployment groups agent on your Linux server:

1.  Change the target from Windows to **Linux**.
2.  Check the **Use a personal access token** checkbox.
3.  Save the script on a notepad for the next step.

#### 2\. Installing the Agent on the Linux Server

To install the agent, run the script copied from the previous step **in the home directory**, make sure you **run the script with a user that has sudo enabled**.

After installation is finished run go into the **azagent** directory and check the installation by trying to run the agent:

```
cd azagent  
./run.sh
```

If the agent returns a **_Listening for Jobs_** message, that means the installation was successful. The next step is to make a service that can run the agent in the background.

To start to install the agent as a service, run:

```
sudo ./svc.sh install
```

This will install the service, and then run it in the background. To stop the service run:

```
sudo ./svc.sh stop
```

Otherwise, if we want to start an installed service, run:

```
sudo ./svc.sh start
```

![](https://cdn-images-1.medium.com/max/800/0*vkJRjhEYQVPeemFc)

Check if the service is running by opening the Deployment Groups tab in Azure DevOps. If it is **Online** then your agent installation is successful and it is available for jobs.

This short installation guide was based on an article by [Microsoft](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=azure-devops).

### Configure your Linux server for automation

Now, this would be the fun part. Microsoft already did all the heavy lifting with the boring installation things. Now it is our turn to automate things.

#### 1\. Make a script to automate NGINX configurations

To add new NGINX configurations we will create a shell script, add this _add-nginx-conf.sh_ script on the home directory:

```
#!/bin/bash
conf_path="/etc/nginx/conf.d/$1.conf"

if [ -f "$conf_path" ]
then
    conf_text=`cat $conf_path`
    if [[ "$conf_text" == *"$2"* ]] || [[ "$conf_text" == *"localhost:$3"* ]]
    then
        echo "Proxy route or localhost port has been used, please manually reconfigure your Nginx configuration."
    else
        word="\n\n\tlocation \/$2\/ {\
\n\t\tproxy_pass http:\/\/localhost:$3\/;\
\n\t}"
        match="# Insert here"
        echo "$conf_text" | sed "s/$match/&$word/g" > "$conf_path"
    fi

    nginx -t
    systemctl reload nginx
else
    conf_text="server {
	listen       80;
	listen       [::]:80;
	server_name  $1;

	# Load configuration files for the default server block.
	include /etc/nginx/default.d/*.conf;
	
	# Insert here

	location /$2/ {
		proxy_pass http://localhost:$3/;
	}

	error_page 404 /404.html;
		location = /40x.html {
	}

	error_page 500 502 503 504 /50x.html;
		location = /50x.html {
	}
}"

    echo "$conf_text" > "$conf_path"

    chcon unconfined_u:object_r:httpd_config_t:s0 "$conf_path"
    chown root:root "$conf_path"

    nginx -t
    systemctl reload nginx
fi
```

This script will create a search for configuration files named using the associated DNS, it will create a new one if it was not found. Once found, the new configuration routes will be added there. Mind you, the configuration route in this script would be a basic proxy pass, you might want to add more configurations to the script if you want to use it for production.

The shell script accepts three parameters. The DNS, proxy route, and the port being used:

```
sudo ~/add-nginx-conf.sh <DNS> <proxy route> <app port>
```

Example:

```
sudo ~/add-nginx-conf.sh yourdomain.com enrichment_track 14321
```

In this example, the script will add configurations for the yourdomain.com domain, with a reverse proxy to port 14321 in the enrichment\_track.

#### 2\. Make a script to automate system service configurations

To add new system service configurations we will create a shell script, add this _make-service.sh_ script on the home directory:

```
#!/bin/bash
file_path=`pwd`
service_path="/etc/systemd/system/$1.service"
if [ -f "$service_path" ]
then
    echo "Service file already exists"
else
    touch "$service_path"
    service_contents="[Unit]
Description=.NET Web API for $1

[Service]
WorkingDirectory=$file_path/_$1/drop/s
ExecStart=/usr/bin/dotnet $file_path/_$1/drop/s/$2.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=<user>
Environment=ASPNETCORE_ENVIRONMENT=<server environment>
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target"
    echo "$service_contents" >> "$service_path"
    systemctl start $1.service
    systemctl enable $1.service
fi
```

**Before you copy and paste the code, notice the `<user>` and `<server environment>` tags in the script. Please change it to your corresponding user and server environment beforehand. Server environments in this case would be either Development or Production.**

The script will generate new systemd service files that would run .NET Core projects. To generate scripts for other types of applications you would want to change the given template.

The shell script accepts two parameters. The service name, and the project name used:

```
sudo ~/make-service.sh <service name> <project name>
```

Example:

```
sudo ~/make-service.sh CI-Build API-Project
```

In this example, the script will add a new system service file _CI-Build.service_, that runs a dll file named _API-Project.dll_.

#### 3\. Setup a user to run systemctl and Nginx commands without password prompts

For automation purposes, the user that you used to install your Azure Agent needs to be able to run systemctl and Nginx commands without a password prompt. To remove the password prompts on sudo run:

```
sudo visudo
```

We need to run all the commands in the script using sudo without having a password being prompted. So, we make exceptions for those commands, and the corresponding scripts we are going to use.

Then add this on the last line _(change the `<user>` tag beforehand)_:

```
<user> ALL=(ALL) NOPASSWD:/usr/bin/systemctl, /usr/sbin/nginx, /usr/bin/chcon, /usr/bin/chown, /usr/bin/echo, /usr/bin/touch, /usr/bin/sed, /home/<user>/make-service.sh, /home/<user>/add-nginx-conf.sh
```

Exit and save the config file, then try running one of these commands using sudo. You should not be prompted for a password.

A full explanation on how to run a sudo command without a password can be seen [here](https://www.cyberciti.biz/faq/linux-unix-running-sudo-command-without-a-password/).

### Prepare the Pipelines

#### Prepare the CI Pipeline

![](https://cdn-images-1.medium.com/max/800/1*fRlXDaaNIGDMUXlx8ER1gg.png)

In this tutorial, we are going to use a .NET Core app as our example.

1.  First, go to the Pipelines tab.
2.  Click **New Pipeline**.

![](https://cdn-images-1.medium.com/max/800/0*lwYKW8-orAtqp1xr)

Use the classic editor.

![](https://cdn-images-1.medium.com/max/800/0*w9fgiTigSL8dU4Pu)

Choose a repository and click **Continue**.

![](https://cdn-images-1.medium.com/max/800/0*j8MZijq5kfQ3gFyo)

Apply the .NET Core template.

1.  Search for **“.NET Core”**.
2.  Then apply the template

![](https://cdn-images-1.medium.com/max/800/0*aJDSwZlIsa-uVRjl)

Because we are deploying a very small example app, I didn’t prepare any tests so:

1.  Click the Test task.
2.  Remove the task.

Otherwise, if you are using tests, then don’t remove this task.

![](https://cdn-images-1.medium.com/max/800/0*i2vTHkZzgc3RK2Op)

Finally, this is what you are left with. In the next step, we will implement Continuous Development in our solution.

1.  Change the name of your CI pipeline.
2.  Save the Pipeline.

#### Prepare the CD Pipeline

![](https://cdn-images-1.medium.com/max/800/0*j4Oyuueu6v8jcAa-)

To add a new Continuous Development pipeline:

1.  Go to the **Releases** tab.
2.  Click the **New**.
3.  Then click **New release pipeline** to create a new pipeline.

![](https://cdn-images-1.medium.com/max/800/0*PksyAUqhU2tdHkTW)

After clicking the **New release pipeline** button a pop-up will appear, then select **Empty Job**.

![](https://cdn-images-1.medium.com/max/800/0*B8Sh7jl3MZJ4qtF1)

To connect to a CI pipeline:

1.  Click **Add a new artifact**.
2.  Choose the project that contains the CI pipeline.
3.  Choose the CI pipeline.
4.  Give an alias to your artifact.
5.  When all is done click **Add** to finish the configuration.

![](https://cdn-images-1.medium.com/max/800/0*38Fjnw3zUuPhvji9)

Then for the Continuous Deployment pipeline:

1.  Click the thunderbolt icon.
2.  Then check the **Enabled** button.

![](https://cdn-images-1.medium.com/max/800/0*0iqhBSovBPD7xVpL)

After creating a stage, on **stages**, double click on tasks to go to the **Tasks** section.

![](https://cdn-images-1.medium.com/max/800/0*ojZ6-NrtHwo0Aeie)

In the tasks section, make a deployment group job.

1.  Click the three dots to show options.
2.  Add a deployment group job.

![](https://cdn-images-1.medium.com/max/800/0*FyUB5L1Iei_pTuQN)

Choose the Linux deployment group we made on [this step](https://docs.google.com/document/d/1KauINvdb7BFPz9pdlGhwGw93Jj7unkbnTNWAXrstOeg/edit#heading=h.n6j71fd5yi6b).

![](https://cdn-images-1.medium.com/max/800/0*3XuKy89xJJyYXec8)

Add a task to the deployment job.

1.  To add a new task click the **+** sign.
2.  Then search for **Download build artifacts**. This task is to download artifacts from the CI pipeline to the Linux VM.
3.  Finally, **Add** the task.

![](https://cdn-images-1.medium.com/max/800/0*7r1MUEHrP4G5AbB_)

Next is to configure your Download Build Artifacts job. Follow these 6 steps.

1.  Click the **Download Build Artifacts** task.
2.  Choose to download artifacts by **Specific Build**.
3.  Define your Azure DevOps project
4.  Define your CI pipeline.
5.  Check the **When appropriate download artifacts from the triggering build** checkbox to trigger a download only after the build is finished.
6.  As for the download, type choose **Specific files**.

![](https://cdn-images-1.medium.com/max/800/0*Y5iGYaSEcE2O1u1y)

Then search for the **Command line** task:

1.  Create a new task by clicking the **+** button.
2.  Search for **“command”**.
3.  Click **Add** to add a new task to the existing job.

![](https://cdn-images-1.medium.com/max/800/1*EWRJZrvelnFdQywZNAAxJA.png)

Then in the command line, paste in the script you will be running:

1.  Copy the **appsettings.json** file from the home directory to the build folder.
2.  Generate system service configuration files and start **if not exist** using the _make-service.sh_ script.
3.  Restart the service for the .NET Core project.
4.  Add NGINX configuration **if not exist** using the _add-nginx-conf.sh_ script.

> Notice in the command line, there are tags like **$(Build.DefinitionName)**. This is one of Azure Pipeline’s pre-defined variables you can learn more about it [here](https://docs.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml).

![](https://cdn-images-1.medium.com/max/800/0*sxB70Pit7xBQICV3)

After you’re done with the configuration, save your Release pipeline:

1.  Change the name of the Pipeline
2.  Save the pipeline.

### Testing the pipeline

To test the pipeline, go to the Pipelines section, open the pipeline you just created, and click **Run Pipeline**.

![](https://cdn-images-1.medium.com/max/800/1*ulEJaZGiPYdzOIEoOt8iWw.png)

We run it from the CI pipeline because both CI and CD pipelines are already connected.

### Summary

In this tutorial, you have learned how to automate CI/CD deployments using Azure DevOps and some shell scripts. We tested it out using a .NET Core project. The Linux server used was a Centos 8.

To recap what we have done:

*   Installed an Azure Agent on Linux.
*   Configured our Linux server for automation.
*   Prepared the CI/CD Pipelines.

You now have an open playbook to use when you want to automate deployments. Hope this tutorial was of help and have a nice day!