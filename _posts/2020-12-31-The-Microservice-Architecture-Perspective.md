---
title: The Microservice Architecture Perspective
description: >-
  How sheer numbers can become high performing solutions, an explanation on
  Microservices.
date: '2020-12-31T16:51:07.316Z'
layout: single
author: Agustinus Theodorus
categories: ['tech']
keywords: []
slug: >-
  /the-microservice-architecture-perspective
header:
  teaser: https://cdn-images-1.medium.com/max/800/0*VzeqoAS5yZrIpeZQ
comments:
  provider: "disqus"
  disqus:
    shortname: "agustinustheo"
---

Making software that serves more than a couple of thousand users can be hard. The difficulty is not in making the app itself but in how we make the app reliable. Microservices is a solution to messy problems like these. Instead of relying on one large server, we use a bunch of small ones to solve our problem. To elaborate on this subject further here are a few perspectives I have on the microservices architecture.

![](https://cdn-images-1.medium.com/max/800/0*VzeqoAS5yZrIpeZQ)

### One Down, Many More To Go

In my honest opinion, one of the reasons I go with a microservice is the reliability factor. With multiple servers, this can be the case. We add reliability to the service by adding more of them. This technique is called [load balancing.](https://www.citrix.com/en-id/glossary/load-balancing.html#:~:text=Load%20balancing%20is%20defined%20as,server%20capable%20of%20fulfilling%20them.)

![](https://cdn-images-1.medium.com/max/800/0*trVIG9l2V7inAkov.png)

Instead of burdening all the traffic to one server, you can balance it to multiple servers. So it can be nearly impossible to down the service. The best part of this is that you don’t need to have expensive high throughput servers to make this happen. For example, you can host your server on a few [digital ocean droplets](https://www.digitalocean.com/products/droplets/), instead of one large one.

Though load balancing alone cannot determine a microservice, there are other characteristics that would need to be fulfilled. This is but only one of the advantages of the microservice pattern.

If you are using a monolith pattern logically you can still use load balancing, the only downside would be that you would need multiple high throughput servers to serve your app. Multiplying the cost.

### Pay For What You Need

Before I start, let me say. This section is debatable. I am explaining this from my point of view and my previous experiences. Because when your entire service is determined by small services, it can be easy to cut costs. We can buy only the things that we need.

![](https://cdn-images-1.medium.com/max/800/1*rTd5CdXZV626QTVDZuK_EA.png)

For explanation purposes let’s say a 1 GB droplet can handle about 1k users. So to handle 5k users you probably would need to use the 8GB droplet for the price of $40 a month.

Instead, you can use 3 droplets. Two 1 GB droplets and one 4 GB droplet for $30 a month. We buy two 1 GB droplets so we can use one for the service and use the other one for load balancing. With this scheme, you saved $10 a month.

This is a very simple example. A better one would be when we have separate microservices that connect to each other, we can tune the prices to our exact use of computing power.

For example, the donuts API may have more load than the coffee API. We can then scale up the donuts API server to be able to handle those requests. Being able to specify upgrades by service can help reduce costs tremendously.

### Separate Your Interests

Monoliths have this habit of merging all services together, related or not. This causes a problem when one of the services goes down, it will take the entire app down with it. If you value reliability, you wouldn’t want that.

Using the microservice architecture each service can be separated, making it independent of one another. This type of API development is called domain driven development or DDD. It is a very interesting way of designing systems small that can scale big. [You can read more about DDD here](https://www.confluent.io/blog/microservices-apache-kafka-domain-driven-design/#:~:text=Microservices%20have%20a%20symbiotic%20relationship,that%20makes%20the%20system%20work.). Reading more into this topic might lead you to cool system design patterns like [CQRS](https://microservices.io/patterns/data/cqrs.html), [Database per service](https://microservices.io/patterns/data/database-per-service.html), and [API Composition](https://microservices.io/patterns/data/api-composition.html).

![](https://cdn-images-1.medium.com/max/800/1*kKsAICXGDY_KbwOpgxSaBQ.jpeg)

The picture above explains my point perfectly. Connections from the client would be made to a gateway/edge API. The edge API would then connect to the multiple microservices available for each specific request. With the corresponding databases behind each specific API.

Say, the user wants some books, of course, the data would be retrieved from the books API. After seeing the catalog of books, the user made up their mind and wishes to check out. The user is then connected to the check out API. Notice that all the services are independent of each other it does not coincide with one another other than the edge API. It adds reliability to the app.

### Scale More!

Why does this section have an exclamation mark? Because this is the main gist of using microservices. It is the best feature anyone could have asked for. Put reliability and scalability together and what do you have? A great user experience.

Not all companies might need the blessing of microservices but for those that do it really does make a huge difference. All the sections before this one support this. Separable domains, cost-effectiveness, and reliability.

But you do not need to scale everything. When you are building a team of services using microservices will help you in the long run, but when you are making an MVP or some other small apps you would not need to implement microservices at all. It might even slow you down!

Microservices is not the grand bullet for everything, even though it has both reliability and scalability to its name. It has good things to offer for select cases, not for everyone.

### Small and Compact

Microservices like the name entails are very small apps that communicate with each other. With specificity in mind, teams can deploy new services quickly without breaking another.

This coincides perfectly with agile teams that want to reach a certain target. Deployments are easy because it allows for easy and straightforward testing. You only need to test the things that you deploy, there is no need to test the other services along with it.

Almost all the articles I’ve read about microservices applaud the ease of deployments this architecture has to offer. Though that’s not all the benefits of being small and compact.

Maintainability wise, it will be much easier than monoliths. The functions are simple because the folders aren’t merged into one project. Honestly from my own experience handling a monolith, traversing the folders was very difficult. Filenames can sometimes be similar in a way that I confuse which service is which.

Sure, people can say that a confusing monolith project reeks of bad code management. When you have more than 100 features in a project it does tend to get a little messy. Add to the number of teams responsible for different features contributing to the **same codebase** 🤯.

> I can only imagine the number of branches that repo has…

### More Autonomy and Faster Development Speeds

With microservice, this doesn’t have to be. Microservice brings more autonomy to the team. Working together between teams is not easy. We have our own schedules that we need to attend. But when we use the microservices concept, the pain of inter-team management becomes invisible.

All you need would be a coding standard, and you would be good to go. Though more autonomy, coupled with faster development could lead to trouble **if you don’t have good documentation**.

Let me make this clear, working autonomously is great and all. But in the end, you still need to communicate. What better way to miscommunicate other than having no documentation. Believe me, I have experienced these kinds of things first hand. Microservices does not solve problems like these.

With more autonomy teams can be more business-oriented with their goals, timelines become much shorter and development can focus more on new features.

### With Great Power Comes Great Documentation

Elaborating from the previous section, a good analogy for microservices would be “With great power comes great responsibility”. I am not kidding, when you have tons of services you would **wish** **for good documentation**. For a good perspective here is an illustration of the Netflix microservice backend.

![](https://cdn-images-1.medium.com/max/800/0*d9cZNb3AX_fOgC-y.jpg)

Now imagine, how much of that would you remember. The answer is none. That’s why documentation is important. Probably using Netflix as an example is a huge exaggeration — _but hey it gets the point across._

Now I know — _every application needs documentation_. That’s true but it becomes double for microservices. There are benefits to good documentation though. The better documented the microservice is, the reusable probability of each service will be increased. This brings us to the next point.

### Reusability

What I love about writing code is how much less of it I have to write. Get what I am saying? Reusable components, reusable APIs, reusable _everything_! I would integrate every one of my apps into one large system, and if I can reuse some parts of it for another app I would do it without notice.

The part that I like about microservices is that it’s very portable, I mean one service can be accessed by many. That’s why I usually try to make microservices as general as I can make it — _doesn’t mean that I won’t make it specific when I need to_. It just comes as a natural thing to do to reuse things you made.

### Conclusion

To summarize microservice architectures are usually used to help increase the reliability of a system. The benefits of using a microservice architecture are as follows:

*   Made to be reliable.
*   Pay for what you need.
*   Write separate applications for each domain.
*   Have a nature to scale.
*   Small and compact.
*   Separated development for teams that wants to move fast.
*   Reusability factor.

Though the microservice architecture does come with its faults. The disadvantages of using a microservice are as follows:

*   With great power, comes great documentation.
*   Small companies with few teams are not suitable for this architecture.
*   Does not fit every business model (e.g monoliths are better for MVPs).
*   Because it has a nature to scale, it can become bloated if not checked. We can’t all be like Netflix.

This was my perspective regarding microservices. I hope it was clear enough of an explanation.