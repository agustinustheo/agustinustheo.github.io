---
layout: single
author: Agustinus Theodorus
title: How To Choose Your First Techstack
description: 'So many frameworks, with so little time. Save yourself the trouble and choose!'
date: '2020-11-18T13:01:31.000Z'
categories: ['tech']
keywords: []
slug: /how-to-choose-your-first-techstack
header:
  teaser: https://cdn-images-1.medium.com/max/800/0*9RPiaNqFjBvZay_Q
---
For all these years in the tech industry. I assure you that I have had my fair share of technology. A multitude of different frameworks aiming to achieve the same thing with their own different takes. Teams that prefer different tech stacks. It doesn’t matter what you learn but how fast can you adapt.

![](https://cdn-images-1.medium.com/max/800/0*9RPiaNqFjBvZay_Q)

In this article, I might not give you **exact** answers to what you should choose. Because I believe that you should find what is comfortable for yourself. I would just explain what my ideas were when I chose my stack.

### The Job Stack

When you first started working as a software engineer, you would realize that you can’t decide what tech stack your company would use, initially. You would need a lot more rep and power to be making those decisions.

So, there you are stuck with a legacy system using [VB 6.0](https://winworldpc.com/product/microsoft-visual-bas/60). Probably that shouldn’t be your choice. If you have ever used [VB 6.0](https://winworldpc.com/product/microsoft-visual-bas/60) damn, how programming has improved over the years. No, this is not an endorsement for [VB 6.0](https://winworldpc.com/product/microsoft-visual-bas/60).

In my company, we mainly use C# and PHP. I was mentored using C# and [ASP.NET MVC](https://dotnet.microsoft.com/apps/aspnet/mvc) when I was first starting, then continued to use PHP and CodeIgniter in my second year. So the choices were between these two.

I chose C#. Why? Well here comes the subjective part. PHP is not a language for me. It is too flexible and there are too many syntaxes that do the same thing. My experience was with PHP5, now it’s with PHP7 things have improved but I have already invested more time on C#.

The point here is that you can choose to deepen your skill in a modern language used by your current employer. It would be easier and you can suggest implementations of some cool stuff using your employer’s language of choice.

For example, recently when being tasked with making a multithreaded background service. Using C# and [.NET Core](https://dotnet.microsoft.com/download) really helped me craft a perfect solution for my employer.

The best part is not the solution, the best part would be the fact that anyone in your team can maintain your application. Collaboration would be easy because the language used is universally understood by engineers in your company.

The only downside would be the lack of freedom. You can’t force your will upon your employer, and if you try to master a different language, you end up learning 2 things at the same time. It depends on you personally but I would prefer to avoid that.

### The Swiss Army Stack

Ooh, swiss army stack! That sounds cool, doesn’t it?

This particular stack consists of mainly one language with multiple different frameworks for different situations. A good example here would be the Javascript/[NodeJS](https://nodejs.org/en/) ecosystem.

#### Javascript

Who here doesn’t know NodeJS? Look it up.

Using a front-end framework like [React](https://reactjs.org/), [Vue](https://vuejs.org/), or [Angular](https://angularjs.org/) has been the norm. They all use Javascript/[Typescript](https://www.typescriptlang.org/). So why bother learning a different language other than Javascript? Just use [Express](https://expressjs.com/) for the backend and your all set. Fullstack Javascript here we go.

> Hey, do you know Javascript can do AI too? It can, there is a library called [Tensorflow.js](https://www.tensorflow.org/js). Now don’t tell me that isn’t amazing?!

To be clear, I am not advocating for Javascript. It is just one of those languages that have a lot of functionality. Another example would be Python, and probably [Dart](https://dart.dev/).

#### Python

I don’t know about you, but Python is still looking hot nowadays. It still hasn’t lost it’s easy to use appeal. You can make a ton of things with Python, web applications, APIs, AI applications, background services, etc.

As far as I know, the Javascript ecosystem’s AI libraries aren’t as mature as Python. Python is the main go-to language for AI. Youtube AI programming tutorials usually are done using Python. Some exceptions are using Javascript but my argument still stands.

> But Python does suck at frontend programming. Javascript beat Python in GUI building by a longshot.

#### Dart

Honestly, this should be an honorable mention. But because the environment is quite interesting and has some potential, I thought I would bring it to light.

So, the main powerhouse behind [Dart](https://dart.dev/) is Google’s [Flutter](https://flutter.dev/). The prospect of a multiplatform frontend framework for Desktop, Mobile, and Web is very promising, to say the least.

> Sure, Javascript has a multiplatform framework. There’s [React Native](https://reactnative.dev/). But it’s only for [React](https://reactjs.org/). So, mobile and web. How about [Electron.js](https://www.electronjs.org/)? Sure it would work for Desktop. But [Flutter](https://flutter.dev/) alone compasses all three.

Backend though. It isn’t as promising. The only backend framework I know that uses Dart is probably [Aqueduct](https://aqueduct.io/). Though to be honest, I haven’t had time to research that. So I don’t know if it is already production-ready.

Out of the three languages, I would say Dart is the least mature tech stack. An immature tech stack can bring opportunities for new developers to contribute to the open-source ecosystem, but the downside would be very unstable ecosystem wise.

### The Mature Stack

Mature by what definition? These tech stacks have been around for a while. Has been battle-tested and many applications are using them.

For example, in the case of web frameworks, a mature framework would be C#’s ASP.NET, Java’s Spring, Python’s Django, PHP’s CodeIgniter, Symfony, and Ruby on Rails. These are some of the oldest tech stacks, developed between 2002–2006. Many corporate applications use these frameworks.

Being knowledgeable about these tech stacks are only good if you want to join large corporations. New startups have a very different tech stack, not because these frameworks are bad. But because there exist more production-ready options today.

I personally have tried 3 of the previously mentioned frameworks. ASP.NET, Django, and Ruby on Rails. **My favorite was Ruby on Rails**. Django was always too complicated for me, I prefer to use Flask for Python web development. It’s hard to host ASP.NET applications on Linux so I also passed on that.

### So, which should I choose?

Now, this is where it gets opinionated.

> Take this with a grain of salt, try it out yourself. There are many more technologies available than what I have written here.

If you dislike your job stack, well you can use it for professional purposes only, and try to find another job with a tech stack you like. Nothing you can do about it.

> My simple guide would be to master one object-oriented language and one scripting language, and if you’re lucky both of them would be multiplatform.

I on the other hand love C# and .NET Core, so it has not been a problem for me. I prefer it over Java because C# libraries are helpful and are already all too familiar to me. .NET Core is also multiplatform, and unlike previous ASP.NET releases, it runs seamlessly on Linux.

My scripting language of choice would be Python. Writing in Python has been so wonderfully easy. I am very comfortable with the amount of flexibility the language gives me. The libraries are also mature and because I have an obsession with AI, implementing an AI model in a web application is seamless.

Seeing my choices you might be wondering. “Hmm, so C# and Python. Aren’t both of them backend languages?”, and you are right. Well, apart from C# Blazor. But I don’t use Blazor that much so I’ll turn a blind eye to that.

For front-end purposes, I use Javascript frameworks. This limits my capabilities to pure web applications because Javascript frameworks are mostly well known for its front-end web frameworks. But another benefit of learning Javascript is that it is an in-demand skill. Javascript can have production-ready environments for both front-end and back-end most companies take advantage of that.

Furthermore, Javascript has another benefit of being **a free tech stack**. Here’s what I mean. There is a service called [Netlify](https://www.netlify.com/). Netlify hosts JAM Stack applications, and if you have reasonably small traffic, Netlify covers the costs for free. JAM stands for Javascript, APIs, Markup. Add to that Netlify also has a service called [Netlify Functions](https://www.netlify.com/products/functions/).

![](https://cdn-images-1.medium.com/max/800/1*Sjwe6yN2B25zJI91MAqMmA.png)

Netlify Functions utilize serverless technologies to make simple APIs using Javascript using [AWS Lambda](https://aws.amazon.com/lambda/) as their base. You are only charged after 125k~ requests or so.

![](https://cdn-images-1.medium.com/max/800/1*wl90hwJrFDI7-UW1jHQfjQ.png)

My next statement might sound controversial, but in my opinion, adding Javascript to your knowledge repository can help you get a job. It is very popular and very flexible. Being multiplatform it is cost-effective and the choice of many modern startups. Some corporations are also planning to migrate to Javascript environments.

### Conclusion

So? Found your calling yet? Don’t rush. It’s fine. Do what you think is right. If you want to learn them all, be my guest. I am already at a stage where I want to master concepts rather than frameworks so I can say I understand how it feels. Be slow but sure, you won’t master it all overnight.

> “The master has failed more times than the beginner has even tried.” — [Stephen McCranie](https://www.goodreads.com/quotes/1252243-the-master-has-failed-more-times-than-the-beginner-has)

Don’t be afraid to try. Failure is only a step. Thank you and have a nice day!