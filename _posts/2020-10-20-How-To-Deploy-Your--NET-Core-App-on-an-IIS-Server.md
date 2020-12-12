---
layout: single
author: Agustinus Theodorus
title: "How To Deploy Your\_.NET Core App on an IIS Server"
description: "A quick 5 minute tutorial on how to deploy a\_.NET Core app on an IIS server."
date: '2020-10-20T13:51:54.780Z'
categories: ['tech']
keywords: []
slug: /how-to-deploy-net-core-on-iis
header:
  teaser: /assets/img/0__QkuSb8wVpC5wY7kj.jpg
---

When you work in an enterprise environment you can't choose where you would want to publish your application, especially if you enter a very established company. There would be times that you have to publish in a Linux environment, but in this case publishing in a Windows environment.

![]({{ site.url }}/assets/img/0__QkuSb8wVpC5wY7kj.jpg)

### Configure your .NET Core project

When trying to deploy on an IIS Server, make sure you already configure your Startup.cs and Program.cs accordingly. On the **Startup.cs** you should add this config:

![]({{ site.url }}/assets/img/0__WH3WzOX03__rPBePK.jpg)

On the **Program.cs** add this config:

![]({{ site.url }}/assets/img/0__7ndyVaQEnF108l4V.jpg)

### Configure your IIS Server

#### Downloading the required packages

Install the required packages before configuration.

*   [Microsoft Visual C++ 2015 Redistributable Update 3.](https://www.microsoft.com/download/details.aspx?id=52685)
*   [Windows Server Patch KB2533623.](https://support.microsoft.com/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)

#### Downloading the dotnet SDK

Deployments only need runtime, but if you want to have a more flexible environment that allows you to build and publish **dll** files rather than only running it you would want to install the latest dotnet SDK, [here](https://dotnet.microsoft.com/download/dotnet-core).

#### Install the Azure Artifacts Credential Provider

Next, install the Azure Artifacts Credential Provider, full instructions can be seen [here](https://github.com/Microsoft/artifacts-credprovider).

#### Azure Artifacts Credential Provider Manual Installation Guide

Find the latest release [here](https://github.com/Microsoft/artifacts-credprovider/releases), and download the .zip package.

![]({{ site.url }}/assets/img/0__tlJiw8J58KyBZk1D.jpg)

After downloading the zip archive, copy the plugins folder to **%USERPROFILE%/.nuget**

![]({{ site.url }}/assets/img/0__DNrZNycYxJ7EmmAK.jpg)

Set your environment variables, open **User Variables** and create a new entry.

![]({{ site.url }}/assets/img/0__jPaoP30GSXaE__mvK.jpg)

Set the variable name to **NUGET\_PLUGIN\_PATHS**, and because in this case, we are going to be using the dotnet credential provider, so set the value to

**%USERPROFILE%\\.nuget\\plugins\\netcore\\CredentialProvider.Microsoft\\CredentialProvider.Microsoft.dll**

![]({{ site.url }}/assets/img/0__1IbSM3kmog2__9kWl.jpg)

Then finish the setup by clicking **OK** on the **Environment Variables** dialog.

### Publish your .NET Core application

#### Pull the project from DevOps

Get into the directory where you want to put the application.

![]({{ site.url }}/assets/img/1__JdLjczILQMnVA9QutYSYog.png)

Get inside the newly formed folder and run the dotnet publish command. Further info regarding the commands can be seen [here](https://docs.google.com/document/d/14YRTCJ8XO7yumjwmwyN67ZrEpc4LDrbg6UXlGC4noI0/edit#heading=h.eyphkxwv1b7).

![]({{ site.url }}/assets/img/1__bVpZ052xiPvEIl34UfVtfg.png)

If an authorization prompt to [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) pops up, login using the [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/) account and enter the code shown in the window.

Otherwise, it should show a successful build like so:

![]({{ site.url }}/assets/img/1__9x3fssWVV25Ks0JUIHOaWw.png)

In this case, **D:\\deploy\\api.test\\bin\\Release\\netcoreapp3.1\\publish** is the **_physical path_**_._

This will be the path used when configuring your app on an IIS application.

### Run your .NET Core on IIS

#### Add the application pool

Right-click on the **Application Pools** option.

![]({{ site.url }}/assets/img/0__Ntuy9f755WVik0zY.jpg)

Choose the **Add Application Pool** option

![]({{ site.url }}/assets/img/0__KW3cmVX3rTQ7Lwjs.jpg)

Name your application pool and set the .NET CLR version to **No Managed Code.**

![]({{ site.url }}/assets/img/0__yZqj4kWhU7wpVDp1.jpg)

Then click **OK**.

#### Add the IIS website (Optional)

**_This configuration is optional as you can use a pre-existing site._**

Right-click on the **Site** option

![]({{ site.url }}/assets/img/0__fOXUHP0QgMyt__NXO.jpg)

Click **Add Website**

![]({{ site.url }}/assets/img/0__ZFzoqMNSIj7Mmwb5.jpg)

Set the **Site name** and the physical path to the .NET Core build.

![]({{ site.url }}/assets/img/0__IHxAaO__VohU2op1x.jpg)

Click the **Connect as…** button, to connect as a specific user. Set your user credentials and click **OK**

![]({{ site.url }}/assets/img/0__hqYDTod9W9vsmTgM.jpg)

Click **Test Settings** and it should show a pop up like this

![]({{ site.url }}/assets/img/0__6zwI7X__4N98nhBPn.jpg)

Then on the **Add Website** dialog click **OK** to finish the setup.

#### Add the Application on the IIS website

On the site’s homepage, click on the **View Applications** button.

![]({{ site.url }}/assets/img/0__xZT4AhnRwDe3__EjL.jpg)

Click on the **Add Application** action

![]({{ site.url }}/assets/img/0__dInCu25X3VsVexlZ.jpg)

Set your alias, this will be the path on the Url. Then, set the physical path to the .NET Core build.

![]({{ site.url }}/assets/img/0__n0yJiu4l94muBAXB.jpg)

Next, select the application pool you previously made, then click **OK**

![]({{ site.url }}/assets/img/0__RWYhIfs7zUhkEePs.jpg)

Click the **Connect as…** button. Set your user credentials and click **OK**

![]({{ site.url }}/assets/img/0__60abQOP6lDndEYGS.jpg)

Click **Test Settings** and it should show a pop up like this

![]({{ site.url }}/assets/img/0__yDHFWEiPupCSrfsL.jpg)

Then on the **Add Application** dialog click **OK** to finish the setup.

When all steps are done, your application should be running. Test this by opening a browser and put **localhost/\*your application alias\*** in the search bar.

### Summary

In this tutorial, you have learned how to serve a .NET Core application using IIS.

To recap what we have done:

*   We have configured our .NET Core web app before deploying it on IIS.
*   We have installed the prerequisites for the IIS server.
*   We installed the required dotnet SDKs and the corresponding Azure authentication providers.
*   We have configured IIS to deploy our .NET Core app.

You now have an open playbook to use when you want to deploy dotnet applications on IIS. Hope this tutorial was of help and have a nice day!