---
title:  "Design Components: Caching"
date:   2024-11-16 08:00:00 -0600
excerpt: "A beginner's intro to caching."
header:
    overlay_image: /assets/images/intro_to_caching.png
---
# Background
To understand caching, we first need to understand that every program needs to access and manipulate data. There are different ways for programs to access data, and each different method has costs associated with it. To keep it simple, in this context, "cost" means time taken to fetch data.

# Data Access Methods
At a high level, programs primarily access data in the following ways:
- Fetching the data from main memory (RAM).
- Getting data from some database.
- Getting data via network call.

## Costs of each Data Access Method
Generally speaking, here is the order of cost for each data access method from highest cost to lowest.
1. **Network call**. Network calls are typically considered to be more expensive than a direct call to the database because we have to connect over the internet. In addition to that, it's likely that whichever process we're connecting to over the internet has to access _their_ database. This means often times a network call is like a database call but with the included overhead and latency of communicating over the network.
2. **Database call**. Fetching data directly from the database is faster most of the time than fetching data over the network. This is because we're not waiting on some external system to access their own database.
3. **Main memory (RAM)**. Generally speaking, the fastest data access comes from RAM.

# What is Caching?
What if we have some process that is constantly fetching the same data over and over again via network calls? We would be wasting a lot of resources waiting on these results. This pattern is more common than you may think. 

**Caching** is the act of taking data received from an expensive data access method (such as a network call) and storing it in a less expensive data access method for faster retrieval. 

## Examples
- Say you have a gallon of tea in the fridge. What if every time you wanted a drink, you had to go to the fridge and drink directly out of the gallon? Instead, you pour some tea in a cup so you don't have to make a trip to the fridge every time you want a drink. In this example the cup is the cache. 
- Your browser caches static web page content. It's likely cached information about this site.

## Data Durability in Caching
One very important thing to understand about caches is that they are typically not considered a durable data source. What this means is that it's not guaranteed that we can recover data if the cache goes down. For this reason, it's important that we always use a durable database as a primary data store in addition to a cache when data loss cannot be tolerated. 

## Cache Miss and Cache Hit
- **Cache hit**: The requested resource is in the cache when we request it.
- **Cache miss**: The requested resource was not in the cache, so we have to fetch it from the database.

## Pros and Cons of Caching

### Pros
- Caching commonly used data can increase throughput and lower latency by avoiding expensive database or network calls, leading to increased scalability and improved user experience.

### Cons
- Caching can lead to stale data.
- Caching is an eventually consistent data source.
- Caching is less durable than database storage and should not be used as a primary data store.
- If a cache goes down, it can cause increased, unexpected load on the database.
- When using a write-back strategy, data loss is possible. 

## Strategies for Keeping the Cache up to Date
As mentioned, one downside of using a cache is the risk of data being stale (i.e. not in sync with the database). There are different strategies for remedying this problem, each with its own tradeoffs. 

- **Write-through strategy**. In a write-through strategy, we update the cache with new information anytime a user makes an update to the database on the corresponding data.
    - Example: say we have a user service, and in it we're caching the user's email. If the user ever updates their email, we would update the cache at the same time we update the database. 
    - **Pros**: 
        - This keeps data consistent between the database and the cache.
        - This minimizes risk of data loss because the data is stored in the database.
        - Ideal for ready-heavy but light write workloads.
    - **Cons**:
        - Writes are more expensive. 
        - We could be wasting storage space for elements that are not being read by the application.
- **Write-back strategy**. In a write-back strategy, we keep the cache up to date with the most recent information and we occasionally write that data to the database, perhaps on a schedule.
    - Example: the user updates their email and we write that data to the cache. Later, we eventually sync the cache's data to the database. 
    - **Pros**:
        - Increased throughput and decreased latency for writes.
    - **Cons**:
        - Risk of data loss if the cache goes down before the data is synced to the database. This strategy should only be used for data where some level of data loss can be tolerated. 
- **Lazy-loading or Cache-aside**. In this strategy, data is loaded into the cache whenever a user makes a request and we can't find that information in the cache. Because it wasn't found in the cache, we would then fetch the data from the database and then update the cache.
    - Example: the user makes a request for their email address. we look in the cache and we don't find it, so we fetch it from the database and update the cache.
    - **Pros**:
        - The cache will reflect what the application frequently makes reads for and nothing extra, keeping the cache small.
        - This approach is low in complexity. 
    - **Cons**:
        - When there's a cache miss, it will take longer to return a response because we will have to read from the database. 
- **Cache invalidation strategies**. We can set a TTL (time to live) on a cached resource. This causes it to be evicted from the cache after the TTL expires. This forces the application to get up-to-date information from the DB. Another example of an eviction policy is a Least Recently Used (LRU) cache. Cache invalidation strategies also help keep the cache small.


## Caching Technologies
The most ubiquitous server-side caching softwares are:
- **Redis**
- **Memcached** 

# Key Takeaways
- Caching is the act of taking data from an expensive data access method (e.g a database call) and storing it in a less durable, less expensive data access method (e.g. main memory (RAM)).
- It increases throughput, decreases latency, and helps with scalability. 
- Caching should not be used as a primary data source if data loss cannot be tolerated. It is most often used in addition to a database. 
- One common issue with caching is stale data, and there are different strategies for helping with this issue - each with their own tradeoffs.