---
title: System Design 101
updated: 2021-01-09 00:00
comments: true
mailchimp: true
image: /images/system_design_cover.png
excerpt: Basic System Design concepts every Software Engineer should understand.
---

You can read this post in Spanish [here](/es/system-design-101).

All my interviews are wrapped [in a little story](storytelling-tips-technical-interviews). And in almost all of them I end up doing a simple question:

> Why is it so important to design our services so they have no state? Specially if they are running somewhere on a server.

![](/images/system_design_question.png)

It is surprising how many people get it wrong. And when they get it wrong, I do a follow up question:

> What is a Load Balancer?

To which almost everybody answers that DevOps/SRE is not their field. This is wrong!

Every Software Engineer (I don't care if you call yourself developer, programmer or unicorn) should understand at least the basics of _System Design_, no matter their seniority or if their specialty is frontend, backend, mobile, productivity tools or [no code](https://www.minimum.run/). 

1. Mostly because it is essential to have a holistic and systemic understanding of how different services and applications interact. 

2. Because the way your infrastructure is set up affects your code. Obviously.

3. And also because it is not that difficult. You don't have to know how to set up a _Load Balancer_ but it is important to understand what it is, why you need one and when to use it.

This is the first post of a series (probably three) that cover the basics of _System Design_. I will not enter in details because it would require to write a whole book about each topic. However, I will provide many resources for you to further read and investigate.

[Share this post on 🐦 twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) or subscribe to my newsletter **With a grain of salt** and receive an email with updates from time to time.

{% include mailchimp.html %}

## A client, a server and a database walk into a coffee shop

The most typical setup that you will find out there is a frontend application (let's say it uses [React](https://reactjs.org/)) that talks to a backend service (let's say it's a [Spring](https://spring.io/) application written in [Kotlin](https://kotlinlang.org/) that serves a [REST API](https://restfulapi.net/) and runs in a [Docker](https://www.docker.com/) container) which in turn is connected to a database (let's say [PostgreSQL](https://www.postgresql.org/)).

![](/images/system_design_basic.png)

But this representation of the system is incomplete at most. If not totally unacurrate. Thruth being told, there is not just one instance of the frontend application. That would be terrible. Instead, each client runs one on their own device. Desktop or mobile. Each one with their own connectivity conditions using different browsers. And obviously, you can't ignore [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/).

![](/images/system_design_dns.png)

This is more accurate but it's still far from reality. At least on the frontend side. So let's talk about [what happens when you type a url into a browser and press Enter](https://medium.com/@maneesha.wijesinghe1/what-happens-when-you-type-an-url-in-the-browser-and-press-enter-bb0aa2449c1a).

## So you press Enter

Nowadays people just connect their [GitHub](http://github.com/stanete) repository to [Netlify](https://www.netlify.com/) or a similar service and they're happy. But let's take a step back and acknowledge that all HTML, CSS, JS files and the other static assets of a web page need to be stored somewhere on the dark corners of the Internet. Netlify takes care of this and much more but not so long ago people needed to build their React application themselves and upload the content to a service like [AWS S3](https://aws.amazon.com/s3/).

But storing that content is not everything. It needs to be served somehow to the users and it turns out [CDNs](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) are very good at that.

So here is how it goes step by step:

1. The browser asks the DNS for the corresponding IP address of the url. That IP address corresponds to the CDN (maybe AWS Cloudfront). 

2. The browser asks for the content to the CDN. If the CDN doesn't have the content in the cache, the CDN requests it from the storage (maybe AWS S3).

3. The storage returns the content to the CDN server. It sometimes includes an HTTP header called Time-to-Live (TTL) which describes [how long the content is cached](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Expiration.html).

4. The CDN catches the content and returns it to the browser. The content remains cached in the CDN until the TTL expires.

5. When another browser asks for the content, the CDN returns it directly until the TTL expires.

![](/images/system_design_cdn.png)

Today you could probably skip all this but you will still find a lot of applications out there that use this setup (probably most of them). So [here is a post explaining how to host on AWS S3 with CloudFront as a CDN](https://paulstamatiou.com/hosting-on-amazon-s3-with-cloudfront/) and [here is another one explaining how to migrate the whole thing to Netlify](https://paulstamatiou.com/moving-from-amazon-s3-to-netlify/). Your welcome.

Unless the web page has like a huge lot of traffic you won't have to worry about scalability issues for some time. But what happens with the backend?

## Put a Load Balancer in your life

You can't controll where the different instances of the frontend application will run. But with backend services is different. Not only can you control how many instances of the service are running at any time. But you can also control the configuration of the server (like RAM or CPU) in which each instance is running.

You can scale the service vertically or scale it up. Mostly when traffic is low. Just add more memory and/or CPU to the server. However, this is naturally limited, and it increases costs very quickly. Memory and CPU are naturally expensive resources.

Horizontal scaling or scale out is more desirable for large scale services. It just means you can spin up as many instances as you want. But this solution comes with a couple of problems. Don't worry, they can be solved. How can you have a single entry point so the design is not leaked to the outside world? How can you balance the traffic load between all the instances? You're right. Using a [Load Balancer](https://www.nginx.com/resources/glossary/load-balancing/).

![](/images/system_design_load_balancer.png)

With this setup, the backend services are unreachable directly by clients anymore. They connect to the public IP of the Load Balancer which evenly distributes incomming traffic and communicates internally through private IPs.

Horizontal scaling comes with additional benefits. If one instance goes offline, traffic will be routed to the remaining instances. If traffic grows rapidly you can automatically spin up as many instances as needed and shut them down afterwards to control costs.

And here it is the initial question.

> Why is it so important to design our services so they have no state? Specially if they are running somewhere on a server.

Assuming horizontal scaling, there is no guarantee a client will hit the same instance of the service twice in row. It's true that depending on the algorithm of the Load Balancer you can have some consistency. But what happens if an instance crashes completely? State would be lost forever. That's why state is stored in a database, which may at some point become a problem because there is only one instance of it. And it'll probably be your next bottleneck.

## [One of the two hard things](https://www.karlton.org/2017/12/naming-things-hard/)

> There are only two hard things in Computer Science: cache invalidation and naming things. -- Phil Karlton

You can remove some load from the database by using a [cache](https://aws.amazon.com/caching/), which is a temporary data store layer, such as [Redis](https://redis.io/topics/introduction) or [Memcached](https://memcached.org/). It usually stores the result of expensive responses or frequently accessed data. It is much faster than the database because it stores the data in memory. You must be careful though: if a cache server restarts, all the data is lost.

How it works? 

1. After receiving a request, the backend server first checks if the cache has the available response. 
   
2. If it hasn't, it queries the database, stores the response in cache, and sends it back to the client.

3. Next time a client asks for the same data, it is returned directly from the cache without quering the database. 

![](/images/system_design_cache.png)

You better use cache when data is read frequently but modified infrequently. And don't forget to implement an expiration policy. Once the data is expired, it is removed from teh cache. The expiration policy helps to keep the cache data in sync with the database but it does not solve the consistency problem. Invalidating cache when needed is challenging and is not a topic I'm willing to write about now.

## The clone wars

Cache is good and all that but at some point it won't be enough. The database is still a single instance. A [single point of failure](https://en.wikipedia.org/wiki/Single_point_of_failure) and a bottleneck.

Read operations are usually more frequent than write operations. So you can use [replication](https://en.wikipedia.org/wiki/Replication_(computing)) which is a technique for managing multiple instances of a database, usually with a principal/seconday relationship between the original and the copies. The principal database generally supports only write operations while the copies only support read operations.

![](/images/system_design_db_replication.png)

Replication has multiple advantages like better performance, reliability and high availability. The read operations are distributed across the secondary databases, data is never lost because it is replicated across multiple locations and even if one secondary database goes offline you can still access data from other locations.

If all the secondary databases go offline, read operations will be directed to the principal database teporarily. Whenever a secondary database is up and running, read operations will again be redirected to them.

If the principal database goes offline, a secondary database will be promoted to be the new principal database. A new instance will replace the old secondary database. This is more difficult than it sounds because in real life data may or may not be syncronized.

If all the database instances, principal and secondary, go offline, you have a huge problem.

Replication is just one of many techniques for scaling the database horizontally. [Sharding](https://www.digitalocean.com/community/tutorials/understanding-database-sharding) is another one but I will talk about it in another post.

## The magic always happens asynchronous

Some of the tasks your services execute, like sending a notification or processing a photo, don't need to happen right away. They can be executed asynchronosuly. However, the instructions for knowing what task to execute need to be stored somewhere until a service is ready for executing it. For that you can use a [task Queue](https://redislabs.com/ebook/part-2-core-concepts/chapter-6-application-components-in-redis/6-4-task-queues/) also called a [message Queue](https://aws.amazon.com/message-queue/). In reality there are [differences between a task Queue and a message Queue](https://stackoverflow.com/questions/10075817/message-queue-vs-task-queue-difference) but I'm going to ignore them for now. You may have heard of [RabittMQ](https://www.rabbitmq.com/), [AWS SQS](https://aws.amazon.com/es/sqs/) or [Celery](https://docs.celeryproject.org/en/stable/).

![](/images/system_design_queue_workers.png)

The basic architecture of a Queue is simple. Some services, called producers or publishers, create messages and publish them to the Queue. Other services, called consumers or subscribers, connect to the Queue and consume the messages one by one to perform the needed tasks.

> There are only two hard problems in distributed systems:  2. Exactly-once delivery 1. Guaranteed order of messages 2. Exactly-once delivery.

Using e message Queue comes with its own problems. It is difficult to ensure that a message will be consumed [exactly once](https://bravenewgeek.com/you-cannot-have-exactly-once-delivery/). It is equally difficult to ensure all the messages will be consumed in the desired order.

## A new beginning

Here is a more complete and hollistic view of the system and how the different services interact. In terms of scalabiliy, your product needs to be very successful so you need more than this. But I haven't discussed yet authentication, security or rate limiters.

![](/images/system_design_complete.png)

In the diagram you can see a bunch of monoliths: one for the frontend and other for the backend. I still need to talk about the implications of having multiple services talking to each other when breaking the monoliths, backends for frontend, microfrontends or server side rendering. In next posts I will cover those and other topics such as multiple data centers, virtual private networks or sharding.

[Share this post on 🐦 twitter](https://twitter.com/intent/tweet?text={{page.title}}&url={{site.url}}{{page.url}}&via={{site.twitter_username}}&related={{site.twitter_username}}) or subscribe to my newsletter **With a grain of salt** and receive an email with updates from time to time.
