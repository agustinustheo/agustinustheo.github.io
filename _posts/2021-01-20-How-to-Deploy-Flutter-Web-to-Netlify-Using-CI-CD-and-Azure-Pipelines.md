---
title: How to Deploy Flutter Web to Netlify Using CI/CD and Azure Pipelines
description: >-
  From Firebase to Netlify, a step-by-step guide to deploy Flutter web to
  Netlify using Azure Pipelines
date: '2021-01-20T14:44:42.864Z'
layout: single
author: Agustinus Theodorus
categories: ['tech']
keywords: []
slug: >-
  /how-to-deploy-flutter-web-to-netlify-using-ci-cd-and-azure-pipelines
header:
  teaser: https://cdn-images-1.medium.com/max/800/0*Zuxm0Houm1FyBdTE
comments:
  provider: "disqus"
  disqus:
    shortname: "agustinustheo"
---

Developing web apps has become easier over the years. With Flutter it became very easy and fun. But to be honest, deploying it is sometimes a pain. Most services are not ready for Flutter projects being hosted on the web. Sure, **_some are_** but it needs a bit of tinkering.

![](https://cdn-images-1.medium.com/max/800/0*Zuxm0Houm1FyBdTE)

> I don’t mind tinkering a bit, I mean… Why not?

Here in this tutorial, I am going to share with you how to implement CI/CD on your Flutter web deployments using Azure DevOps Pipelines. And best of all, we are going to use Netlify! Why do I sound excited? Because honestly, Netlify is very easy to use. Now, without further ado let’s get to it.

### Installing Dependencies

Before we start, we need to install the required dependencies beforehand.

#### 1\. Installing the Flutter plugin

Well, you can use the [Flutter plugin for DevOps](https://marketplace.visualstudio.com/items?itemName=aloisdeniel.flutter) a dev already made the tool for us. Thanks, [Aloïs](https://marketplace.visualstudio.com/publishers/aloisdeniel) for doing the heavy work! We can use this tool for our purpose.

![](https://cdn-images-1.medium.com/max/800/1*ngPggahl55mBGRJ-FGlSRg.png)

After clicking that button, you would be shown on this screen:

![](https://cdn-images-1.medium.com/max/800/1*52i3t0Qhvy239I_MS3GPpA.png)

Now in what you have to do is:

1.  Choose the **organization** you want to install.
2.  Click install.

#### 2\. Installing the Nelitfy plugin

Install the [Netlify plugin for DevOps](https://marketplace.visualstudio.com/items?itemName=aliencube.netlify-cli-extensions). Thanks, [Aliencube](https://marketplace.visualstudio.com/publishers/aliencube) for doing the heavy work! We can use this tool for our purpose.

![](https://cdn-images-1.medium.com/max/800/1*jv3I_5-xsOTw5rvU9OwA8A.png)

After clicking that button, you would be shown on this screen:

![](https://cdn-images-1.medium.com/max/800/1*MuPn6jiFUqW8KnVkxl4Xww.jpeg)

Now in what you have to do is:

1.  Choose the **organization** you want to install.
2.  Click install.

### Creating your Netlify site

This section will discuss how you can create an empty Netlify site using Netlify CLI. We will be using npm to install it for us. In my case, I use Ubuntu 18.04 as the host OS for the npm installation.

#### 1\. Install Netlify CLI

To install Netlify CLI you just need to run:

```
npm install netlify-cli -g
```

After the installation is complete, check if it’s installed correctly by running this command:

```
netlify
```

#### 2\. Log in to Netlify from the CLI

Before you can start using the CLI, Netlify needs you to authenticate yourself.

```
netlify login
```

Then a browser window will open:

![](https://cdn-images-1.medium.com/max/800/0*4FnW4DUyG4w5_kPC.png)

#### 3\. Creating a blank site

After finishing your login, create a blank site using this command:

```
netlify sites:create --name _<site name>_
```

#### 4\. Retrieving your Site ID and personal access token

To run continuous development remotely you would need both your site ID and personal access token to help you

![](https://cdn-images-1.medium.com/max/800/1*PxWjk8axlVeVwmUt33_gQw.png)

Retrieve your Site ID by:

1.  Going into the site settings.
2.  Copy the API ID value.

The next step would be to retrieve the personal access token.

![](https://cdn-images-1.medium.com/max/800/1*tbk9PgZpJudHMFp6kUvdvg.png)

Go to the User settings.

![](https://cdn-images-1.medium.com/max/800/1*AKe8XmKjoeIzeCfples1MA.png)

To get an access token, you would have to make a new one:

1.  Go to the Applications tab.
2.  Click **New access token**.

Copy and save the access token shown on the next screen, and save it because you will need it for later.

### Creating the CI/CD Pipeline

This part is my favorite because it uses Azure DevOps. I honestly love the pipeline building experience using the UI (e.g the classic editor). But how do you build Flutter projects using Azure DevOps you ask?

#### 1\. Creating the CI pipeline

If you are using Azure DevOps there is no way we can attach a CI/CD pipeline from Netlify’s side as with Github, Gitlab, or Bitbucket. So, we have to get down and dirty and do it ourselves (well with a little help).

![](https://cdn-images-1.medium.com/max/800/1*tlnoTcTVMomfdREqv4-3gg.png)

First, create your pipeline:

1.  Click on **Pipelines** on the left pane.
2.  Open the **Pipelines** directory.
3.  Click **New pipeline**.

Now the beautiful thing about Azure DevOps is they have a UI for all of this, so you don’t need to use YAML at all. Though you can generate YAML code from the UI. Using the classic editor, you don’t have two remember YAML syntaxes and can easily make deployment steps using the visuals.

![](https://cdn-images-1.medium.com/max/800/1*QedkUDLpwccXAdxvYegBYg.png)

You can always configure using YAML in other services, but let’s face it, using UI is so much better. To enable pipeline configurations using the UI, when you are creating your pipeline click the small **use classic editor** link below:

![](https://cdn-images-1.medium.com/max/800/1*kkHm8ht19fXaH29NTHqcyw.png)

After selecting the classic editor, configure your project:

![](https://cdn-images-1.medium.com/max/800/1*eKEEhy8lVmAWILNp5D8kcg.png)

These configurations would be needed to state which project you will be using in the pipeline, start by:

1.  Selecting Azure Repos Git, to select repositories from Azure DevOps.
2.  Selecting your team project.
3.  Selecting the repository you want to CI.
4.  Select the repository branch.
5.  Click continue.

![](https://cdn-images-1.medium.com/max/800/1*Xy65VtOhVJU4j-Td-3Pk7w.png)

Select the **Empty job** button to create a clean slate for our Flutter deployment configuration.

![](https://cdn-images-1.medium.com/max/800/1*TS7XNzRgBE7XTmMSC879Jw.png)

After **Agent job 1** show up you need to:

1.  Click the **+** button to add new tasks in the pipeline.
2.  Search for **Flutter Install**.
3.  Click **Flutter Install**.

![](https://cdn-images-1.medium.com/max/800/1*XGnxKbjSWXkEBuElDjgNAg.png)

After adding Flutter install we can add the Flutter test task to enable testing in our pipeline before being built and deployed. But because I prepared a small app and I am focusing on deployments I wouldn’t be adding testing to the task list.

![](https://cdn-images-1.medium.com/max/800/1*1wCLnwZr7P-flEIcxA3W8g.png)

Next, add two **Flutter Command** tasks to the pipeline. FYI, tasks are forms of stages deliberately visualizing how our CI/CD pipeline connects.

> Let’s face it, using UI is so much better.

![](https://cdn-images-1.medium.com/max/800/1*EFSPGRwA00ZhLFDP72DNrQ.png)

The first **Flutter Command** task will be to enable web configurations (i.e allow builds for web projects).

1.  Click the **Flutter Command** task.
2.  Change the display name to **Flutter Enable Web** (optional).
3.  Add the Flutter arguments to enable web configurations:

```
config --enable-web
```

![](https://cdn-images-1.medium.com/max/800/1*oeQhNtLL0XWYoPA0Uzh51Q.png)

To build the web project, we have to run it manually using the **Flutter Command** task.

1.  Click the **Flutter Command** task.
2.  Change the display name to **Flutter Run Build Web** (optional).
3.  Add the Flutter arguments to build web projects:

```
build web
```

![](https://cdn-images-1.medium.com/max/800/1*xsFcjMbriYuUmwspfakwLw.png)

Add a new task to **Copy files**:

1.  Click the **+** button to add a new task.
2.  Search for **copy files**.
3.  Add the **Copy files** task.

![](https://cdn-images-1.medium.com/max/800/1*JcHH8-BvCC1_pmuI7oDiNQ.png)

After we build the project, build files will need to be copied to the artifacts directory:

1.  Click the **Copy Files** task.
2.  Change the source folder to **$(Build.SourcesDirectory)/build/web**.
3.  Change the target folder to **$(Build.ArtifactStagingDirectory)**.

![](https://cdn-images-1.medium.com/max/800/1*i2HONWrqH04lMo1WEatx0w.png)

Add a new task to **Publish build artifacts**:

1.  Click the **+** button to add a new task.
2.  Search for **publish build artifact**.
3.  Add the **Publish build artifacts** task.

![](https://cdn-images-1.medium.com/max/800/1*K1sbK9jzcue5qpJNITm3iw.png)

After adding the **Publish build artifacts** task you can change the name of the artifact after publishing in this case I changed it to **ci-artifact**.

![](https://cdn-images-1.medium.com/max/800/1*WSvqzb2DlskuGHlrnIE8qw.png)

When all is done, save the CI pipeline.

**TL;DR** **If you want to use YAML you can copy and paste this YAML file here:**

```
pool:
  name: Azure Pipelines
steps:
- task: aloisdeniel.flutter.flutter-install.FlutterInstall@0
  displayName: 'Flutter Install'

- task: aloisdeniel.flutter.flutter-command.FlutterCommand@0
  displayName: 'Flutter Enable Web'
  inputs:
    arguments: 'config --enable-web'

- task: aloisdeniel.flutter.flutter-command.FlutterCommand@0
  displayName: 'Flutter Run Build Web'
  inputs:
    arguments: 'build web'

- task: CopyFiles@2
  displayName: 'Copy Files'
  inputs:
    SourceFolder: '$(Build.SourcesDirectory)/build/web'
    TargetFolder: '$(Build.ArtifactStagingDirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: ci-artifact'
  inputs:
    ArtifactName: 'ci-artifact'
```

#### 2\. Creating the CD pipeline

The continuous deployment section will be used to push our builds directly to Netlify. This part is similar to most JAMStack apps such as React, Vue, Next, and Flutter Web. [An article by Clyde D’Souza](https://codeburst.io/deploying-a-site-to-netlify-with-azure-devops-2743abb61db0) influenced this particular section. But of course, I modified it slightly because the project we’re using is a Flutter project.

![](https://cdn-images-1.medium.com/max/800/1*OygcnF5mrnsRGgLFKBMisg.png)

Open the releases pipeline, click on the Releases tab on the left side of the screen:

1.  Click the **Pipelines** on the left side of the tab.
2.  Open **Releases**.
3.  Click **New**.
4.  Click **New release pipeline**.

![](https://cdn-images-1.medium.com/max/800/1*9jXOfo-ozzfogunzdGCO_A.png)

Create an empty job.

![](https://cdn-images-1.medium.com/max/800/1*H8SF23SAP0kioaI358_YTA.png)

Connect your CD pipeline with your CI pipeline:

1.  Add an artifact.
2.  Select your project.
3.  Choose your CI pipeline
4.  Add the pipeline.

![](https://cdn-images-1.medium.com/max/800/1*4TAHp-48BRxcxNa7805bPQ.png)

Enable the continuous deployment trigger to start after a successful build:

1.  Click the lightning logo to open the trigger page.
2.  Click the **Enabled** button.

![](https://cdn-images-1.medium.com/max/800/1*ZnM9xtSoIvqoer3DITJUmw.png)

Add a new task to the new CD pipeline:

1.  Open your **Tasks** tab.
2.  Click the **+** button.
3.  Search for **Extract files**.
4.  Add the task to the pipeline.

![](https://cdn-images-1.medium.com/max/800/1*Yuz4WMvTXt9tQm_4pj3MZQ.png)

Extract the artifacts according to the build Id.

![](https://cdn-images-1.medium.com/max/800/1*w341jpMUhSmQmu0DVNk3SA.png)

Add a new task to the new CD pipeline:

1.  Click the **+** button.
2.  Search for **Netlify**.
3.  Add the **Install Netlify CLI** task to the pipeline.

![](https://cdn-images-1.medium.com/max/800/1*Z9Vq4cGKp6rd1U0YeKSbZQ.png)

Add a new task to the new CD pipeline:

1.  Click the **+** button.
2.  Search for **Netlify**.
3.  Add the **Deploy Website** task to the pipeline.

To connect our CD pipeline to our Netlify site, we need to have our access tokens and our site ID. We utilize pipeline variables to make it more accessible and provide secrecy to the token.

![](https://cdn-images-1.medium.com/max/800/1*tEpST9H8SwhfhWj8z-NsyQ.png)

Click the **\+ Add** button twice.

![](https://cdn-images-1.medium.com/max/800/1*HKk674mSac5xSASz-rfr2A.png)

You will be shown with 4 input boxes. Put the name of the variable inside the left boxes and the values in the right. For convenience name the access token variable PAT, and name the other variable SiteID respectively.

![](https://cdn-images-1.medium.com/max/800/1*6Qamra986ORQOxI7nLyg8w.png)

Change the deployment configuration:

1.  Click **Deploy to Netlify**.
2.  Insert your access token.
3.  Insert the Site ID.
4.  Select the build source directory.

Your CD pipeline should be finished, save the pipeline and try running it yourself! You can check the dummy site I deployed [here](https://miniature-flutter-milkstore.netlify.app/#/).

### Summary

In this tutorial, you have learned how to Flutter web deployments using CI/CD and Azure DevOps. We then deployed the site on Netlify.

To recap what we have done:

*   Installed the Flutter and Netlify dependencies.
*   Created a blank site on Netlify.
*   Retrieved the authorization token and site ID from Netlify.
*   Prepared the CI/CD Pipelines.

You now have an open playbook to use when you want to automate deployments. In the next tutorial, we will be looking into how we can utilize [Github Actions to make a CI/CD pipeline for Flutter Web and Netlify.](https://link.medium.com/mI4Cq5Y9Acb)