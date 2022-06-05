## Scale From zero to millions of Users (Part 2)

Lets continue the following article.

# Content Delivery Network(CDN)

A CDN is a network of geographically dispersed servers  used to deliver static content, so a CDN always caches(Images, videos, CSS, JS files, ..etc).

in the following figure we can demonstrate the CDN workflow
![Screen Shot 2022-05-31 at 4.09.42 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654006335959/WDhsdl-_n.png align="left")

- User A tries to get image.png by using an image URL. The URL’s domain is provided by the CDN provider. The following two image URLs are samples used to demonstrate what image URLs look like on Amazon and Akamai CDNs:

    • https://mysite.cloudfront.net/logo.jpg

    • https://mysite.akamai.com/image-manager/img/logo.jpg

- If the CDN server does not have image.png in the cache, the CDN server requests the file from the origin, which can be a web server or online storage like Amazon S3.

- The origin returns image.png to the CDN server, which includes optional HTTP header Time-to-Live (TTL) which describes how long the image is cached.


- CDN caches images and send it back to User A. the image remains in cache as long as TTL didnt expire.


- User B sends a request to get the same image.


- The image is returned to User B from cache as long as TTL has not expired.

## Considerations for using CDN

- Cost, most CDN are running by third-party vendors where they charge for everything related to data transfers


- Setting appropriate cache expiry 


- CDN fallbacks in case of failures, where your clients should be able to detect problem and request resources from origin.


- Manual Invalidating files, mostly with the following techiques:

   • Using APIs provided by CDN vendor
     
   • Object Versioning


In Figure 1-11 shows design after adding cache and CDN

![Screen Shot 2022-05-31 at 4.31.44 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654007514160/fMbmXQLtN.png align="left")

# Stateless Web tier

For Horizontal Scaling, We need to store state information(for example user session data) in a separate persistent storage for example NoSQL so seperate web servers can access this data.

# Stateful architecture

Stateful and stateless architectures has some key differences, in stateful server members remember user state from one request to the next

in Figure 1-12 you can check it out

![Screen Shot 2022-06-02 at 2.01.08 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654171406409/0gBKgVsxe.png align="left")

You can see from the following that each user should be routed while authenticating to its corresponding server where it holds its state information. this approach make it difficult to scale application and handle server failures.

# Stateless architecture

In stateless architecture, Users requests can be sent to any webserver which fetch user data from a shared storage, you can see in figure 1-13

![Screen Shot 2022-06-02 at 2.11.07 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654171879139/7Wx2WkafN.png align="left")

stateless tends to be more robust, scalable and simpler.


In Figure 1-14, you can see full architecture with shared data store

![Screen Shot 2022-06-02 at 2.09.50 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654171847534/CzL1yyOdS.png align="left")

# Data Centers

Data centers mostly hold web-tier servers, DB and caches, and requests are Geo-routed based on Data center region by using geoDNS (a service that resolve IP address of domains based on location of user)

so in any case of failure in a certain region, all traffic is routed to another data center.

## Technical Challenges

- Traffic redirection
- Data  synchronization
- Test and Deployment

# Message Queue

Is a durable component, stored in memory that supports asynchronous communication. mostly it serves as a buffer.

It is fairly simple I/O structure

- Input services where they are called producers/publishers create and publish events on the queue

- Output services where they are called consumers where they are connected to the queue and perform operations on events created by publishers.

in Figure 1-17 you can see Message queue approach

![Screen Shot 2022-06-02 at 2.29.06 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654172979169/azTrY3d2_.png align="left")


# Logging, Metrics and Automation

Since your app now is working with a large base of users, investing in tools for logging, metrics and automation is necessary.


- Logging Help identifies errors and problems quickly, we can either view logs per each stack for example server stack, DB stack, ..etc or we can aggregate them to a centralized server for easy searching and viewing.


- Metrics give you business insights or health status about your entire stack for example

   • Host level Metrics: insights about CPU, Memory, Disk I/O, ..etc.

   • Aggregated level Metrics: Performance for entire tier inside your application.

   • Business Metrics: Daily active users, revenue, retention, ..etc.


In Figure 1-19 you can see the updated design

![Screen Shot 2022-06-02 at 2.43.51 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654173857541/hUcHdCOJt.png align="left")


As data grows in your application, its time to scale Data tier


# Database scaling 

We use the same approaches as before, Vertical and horizontal scaling.

Main difference in horizontal scaling that we are using **Database sharding**

## Sharding

Separated large Databases into smaller ones called shards, all shards share the same schema and data is unique for each shard.

The most important part in sharding is the **sharding key** where it is responsible for fetching and modifying data  by routing database queries to the correct database.

in Figure 1-22 you can see users table in a sharded database

![Screen Shot 2022-06-05 at 9.34.42 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1654414518647/tXkq1EVPc.png align="left")


### considerations and complexities for Sharding


- Resharding data if needed, for example a shard can no longer hold data due to rapid growth, also when a shard cannot be evenly data distributed. so in order to solve these issues we need to update sharding function.


- Celebrity Problem, for example a shard that consist of certain celebrities with high user traffic load, this shard will be overwelmed by read operations, so one of the common solutions to add a shard for each celebrity.


- Joins and de-normalization


Hope this article could help you guys, stay tuned for next article about chapter 2



 












