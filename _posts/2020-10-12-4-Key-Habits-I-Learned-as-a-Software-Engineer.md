---
layout: single
author: Agustinus Theodorus
title: 4 Key Habits I Learned as a Software Engineer
description: >-
  Somethings better to know sooner than later, here are the 4 key habits I
  learned as a software engineer.
date: '2020-10-12T16:15:31.910Z'
categories: ['tech']
keywords: []
slug: /4-key-habits-i-learned-as-a-software-engineer
header:
  teaser: https://cdn-images-1.medium.com/max/800/1*u18ToVTMMZ8AN1M4fur0xw.jpeg
---
I am almost 3 years into software engineering. Honestly, I don’t think I am **that** good at it yet (I don’t know if I’ll ever be). But there are times when I wished that I knew somethings sooner than later. I thought it would be fun for me to reflect on those subjects and try to convey them to you as best as I can in this article. So here are the 4 key habits I learned as a software engineer.

![](https://cdn-images-1.medium.com/max/800/1*u18ToVTMMZ8AN1M4fur0xw.jpeg)

### 1\. Write Clean Code (Obviously!)

Bear with me here.

So I haven’t always been the cleanest coder in the team. Through my struggles, I always find it annoying that people don’t understand what I wrote. Heck, I am annoyed that I can’t understand what I wrote a month ago.

I thought, “Hey, why don’t I try to write _cleaner_?”. Well, sometimes cleaner code doesn’t always mean someone else will understand what the code means at first glance. Because there are other factors at play like the business process, etc. But it helps — _I guess._

#### Don’t Repeat Yourself (DRY)

Let me tell you, no one annoys me more than _myself._ I honestly can’t explain how frustrated I am when I read old code that has multiple copies.

> Trying to understand multiple copies of the same lines of code at different points in the documentation is **really confusing**.

So ever since I realized that I tried to write functions for code that I would be reusing often. _Boy did that save a lot of headaches_.

If you don’t do that you might have to change multiple documents that literally have the same line of code just to let’s say — add a new condition to an if-else statement. Take it from me, sometimes you can forget and bugs will happen. So please, write code you going to reuse in a function.

Speaking of functions…

#### Code Purposeful Functions

What I meant by **purposeful** is that you have to write functions that have one exact purpose. For example, I want to hash passwords so I would obviously write a function specifically to hash passwords.

This will add even more readability to your code and it will save you a lot of headaches when you need to say — change the hashing algorithms _or whatever_.

But don’t go too overboard, if you feel that you don’t need to write a function for that **.ToString()** method in C# than don’t. It’s already readable. Don’t overdo it.

### 2\. Learn Design Patterns

Like writing clean code wasn’t enough! Well, to be frank, it isn’t. I have tried to maintain and develop a lot of software in the 3 years I have been a software engineer, and sometimes I would say the default design pattern doesn’t do me any good.

That’s why sometimes micro web frameworks like Flask are my favorites. I can go full creative in how I want my project to be, either by using repository patterns or otherwise.

Find the best fit for you, try to catch up using materials [here](https://refactoring.guru/design-patterns).

### 3\. Automate Tasks

If possible, you should automate everything. Automation might be the best thing I have ever done in my entire career. One time I wrote an application to write APIs for me, so I wouldn’t have to write code anymore.

Well to be fair, it was templated so I still have to code if the app isn’t a simple CRUD application, but it was a good start. This “code generator” coincides with the second point of learning design patterns. I would not have been able to make it if I didn’t implement a design pattern that was comfortable for me.

Another idea would be if your office has a ticketing system of some kind automating those to be added to your Agile boards might be a good idea. Well, it depends, but it is possible.

Other things you might want to automate probably would be menial tasks such as server monitoring and app deployments.

### 4\. Be as Passionate About Writing as Coding

Let me explain, probably your job as a software engineer does not always involve code. Sometimes it will involve writing.

Maybe some companies have KPIs for writing on their engineering blog on Medium. But usually, the writing you would be doing involves documentation.

It is kind of related to coding but rather than writing code, you are writing **about** your code. In my personal experience, I would try to write the best documentation I can in the limited amount of time I would give myself. It helped tremendously when I had to handover projects to other teams.

For me, I write good documentation for myself, because I don’t want to be bothered by other people.

> Good documentation means fewer people asking for your help to understand your code. Which means more time for you.

Keep that in mind the next time your boss asks you to document your code.

### Conclusion

**TL;DR** Write clean code, try to implement the Don’t Repeat Yourself (DRY) mindset to your code. Write purposeful functions that are easy to read. Learn design patterns, find a pattern that you personally like. Also if possible try to automate your tasks. Finally, never be a lazy documentation writer. It can save you more time than you think.

In the end, we all have our own variety of lessons learned. These 4 are just mine. I am writing this not to persuade you rather share with you my personal opinions. Anyway, I hope you have a nice day!