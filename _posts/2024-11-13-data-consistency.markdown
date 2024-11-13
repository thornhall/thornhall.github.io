---
title:  "Data Consistency in Software Systems"
date:   2024-11-13 08:00:00 -0600
excerpt: "How does data consistency apply to software system design?"
---
When designing any distributed software system, one should ask the question "Can my system handle eventual consistency, or is strong consistency necessary?"

How can you figure out which is right for your system? Also, why does it matter?

# Strong Consistency vs. Eventual Consistency
**Strong Consistency** - The guarantee that after a write to some data, any following reads to the same data will reflect what was written. 

**Eventual Consistency** - It may take some time for a write to a system to be reflected in subsequent reads. 

# Why Does it Matter?
Depending on whether you use a strongly consistent or eventually consistent database, different consequences may occur. Here are two examples:

- In banking software, strong consistency is necessary. Suppose John has $50 in his account. He pulls $40 off of his account through an ATM. However, at around the same moment, Netflix attempts to auto-charge him for his monthly subscription (more than he would have after pulling off the ATM). If the underlying database was eventually consistent, the server may still think John has $50 in his account and allow the Netflix charge to go through. This is a simple contrived example, but one could imagine worse scenarios.
- Suppose a celebrity makes a social media post. At around the same time, user A refreshes their timeline and so does user B. User A's timeline does not show the celebrity's post because the database partition their client hit didn't have the latest post information. User B, however, sees the post because their client hit a partition that was updated in time. 

In example 1 we can see that there are much more serious consequences with using an eventually consistent system than example 2. 

# CAP Theorem 
In software systems, CAP Theorem discusses the tradeoffs between 2 properties:

**Consistency**: As discussed, is the notion that all nodes of a system have up-to-date information.

**Availability**: For any given volume and frequency of requests, a response is given. 

The idea is that any given system must optimize for either consistency or availability as they are at odds with each other.

Note that there is a third property (where the "P" comes from), which is **Partition Tolerance**. That is typically a given in distributed systems - the tradeoff is just between consistency and availability. 

Example Systems
Relational databases such as PostgreSQL can be configured to meet strong consistency requirements. 

DynamoDB is optimized for availability over consistency, although it does have configurable consistency options. 

# Key Takeaways
Choosing a strongly consistent or eventually consistent architecture has direct impact on the user experience, and strong consistency is necessary for some cases.

There are tradeoffs to using a strongly consistent system - consistency is often inversely related to throughput. If your application has high availability requirements, you might prioritize that over consistency. 

