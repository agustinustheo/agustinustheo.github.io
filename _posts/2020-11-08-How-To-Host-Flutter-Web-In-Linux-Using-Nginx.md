---
title: How To Host Flutter Web In Linux Using Nginx
description: An alternative to Firebase. DIY on your private server!
date: '2020-11-08T22:02:42.002Z'
layout: single
author: Agustinus Theodorus
categories: ['tech']
keywords: []
slug: >-
  /how-to-host-flutter-using-nginx
header:
  teaser: https://cdn-images-1.medium.com/max/800/0*4HwhJBcQU5Fcjv2U
comments:
  provider: "disqus"
  disqus:
    shortname: "agustinustheo"
---

Thinking of making a web app? I recommend you try Flutter. Flutter’s web capabilities are still in beta but it’s quite promising, to say the least. I have experienced first hand how quick it is to build UIs using Flutter and I think it could soon be a competitive alternative to the Javascript ecosystem such as React, Vue, and Angular.

![](https://cdn-images-1.medium.com/max/800/0*4HwhJBcQU5Fcjv2U)

So to help out the future Flutter community members, a tutorial on how to host your Flutter web app on a DIY private server would help. As a healthy alternative to Firebase.

### What would you need?

#### Umm, first of all, a private server.

With serverless capabilities nowadays it is no wonder that people prefer Firebase, it’s free and it’s painless. Obvious choice.

But what if we want to go independent? Away from the norms? Well, you have to learn it all yourself. Because we are using Nginx, then the server must be a Linux server, not a Windows server.

Assuming you use a Debian/Ubuntu-based Linux distro. Here’s a starter script to help you get started.

```
sudo apt update  
sudo apt install nginx
```

There, I just saved you some googling time. If you want an in-depth guide on Nginx, I suggest you read [this post by Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04).

#### Do we need to install the Flutter SDK on our server?

Umm, yes, and no.

Let me elaborate. It is possible to install the SDK, but why do you need exactly? Installing the SDK helps you to test quick changes to the code on the server to try to fix a few production-related bugs like a cowboy!

![](https://cdn-images-1.medium.com/max/600/0*chBNVmBfQ2j2KpyA.jpg)

I won’t do it though because SDKs consumes a lot of memory and requires a lot of RAM to be compiled. Your private server might not have enough RAM to process it. Believe me, I’ve tried. My VPS only had around 400MB of RAM enough to run a small app but not enough to compile the SDK. **So I didn’t install it**.

So this just leaves one option either you use CI/CD to deploy your Flutter app to your VPS or just use plain old git. By using Git I mean by pushing your builds to the repository. This might not be a best practice for some teams as the entire software world almost unilaterally agree on using CI/CD, but hey — a cowboy’s gotta do what a cowboy’s gotta do.

I am not going into how much of a best practice this tutorial is going to be rather how simple and understandable it is. So let me just leave this choice to you.

#### Do I need a domain?

Do you need a domain? No. It is not required because we are learning **how to host a Flutter web app using Nginx** using a domain or not is not of this tutorial’s concern.

### Build your app

After your app has been tested, run the Flutter build command.

```
flutter build web
```

That should generate a build folder within your project structure. Within the build folder, there is another folder named **web** which contains the essential files to be used for hosting.

The web directory should contain a file named **index.html**.

### Configure Nginx

Here come’s the fun part. Now copy that web directory to your VPS in any fashion you would want. Then make sure you move the build files to /var/www/html.

Let’s say your application name is **todos** so a good folder naming convention would be /var/www/html/todos. Inside the todos directory is the build files, **index.html,** etc. If you’re not familiar with Linux commands this should help, assuming that your build files are in your home directory.

```
sudo mv web /var/www/html/todos
```

Next, you need to go to the Nginx directory on your server more precisely /etc/nginx/sites-enabled. There should be a file named **default**. Open it using a vim/nano text editor of your choice and change the root directory on the configuration file to /var/www/html/todos.

#### No Domain Setup

```
# Default server configuration
server {
       listen 80 default_server;
       listen [::]:80 default_server;

       root /var/www/html/todos;
       index index.html index.htm;

       server_name _;

       location / {
              try_files $uri $uri/ =404;
       }
}
```

Here is an example. The original file should be filled with comments, but I left those comments out in the gist. After changing your config files, check your Nginx config using this command:

```
sudo nginx -t
```

If all is well, then restart the Nginx service:

```
sudo systemctl reload nginx
```

This should restart your Nginx server and get the updated config file. When all is done open up your VPS IP Address from the browser. Your Flutter app should already be up.

I prefer people access my app by domain so I’ll include how to set up the app using a domain here too.

#### Domain Setup

When you don’t want to use a domain, changing the **default** file should be fine (even though some people prefer to leave the default as-is and create a new one).

But when you want to use a domain, make sure you make a new configuration file. The file should have a .conf extension.

I have attached the corresponding gist. The setup should be similar to the previous one with a difference in the server\_name. Then before you open your app from the browser, run:

```
sudo nginx -t
```

If configuration health checks pass, reload the Nginx service.

```
sudo systemctl reload nginx
```

After restarting Nginx, try to open your domain from your browser. It should be done by then.

### Further Note

#### Don’t forget to update domain nameservers

When you want to use a domain, please make sure you point your domain nameservers to your VPS. You are required to do this before any further configuration is to be done.

#### Install an SSL certificate using Certbot

There’s a technology called Let’s Encrypt. It gives you free SSL certificates to be used for your apps, securing your app, and adding HTTPS to your domain. Install Certbot for Nginx using this command:

```
sudo apt install python-certbot-nginx
```

Then, install SSL certificates on your domains:

```
sudo certbot --nginx -d your\_domain -d www.your\_domain
```

### Summary

In this tutorial, you have learned how to serve a Flutter application using Nginx. You now have an open playbook to use when you want to deploy Flutter web applications using Nginx on your own server. Hope this tutorial was of help and have a nice day!