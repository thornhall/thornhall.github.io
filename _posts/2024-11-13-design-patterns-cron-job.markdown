---
title:  "Design Components: Cron Jobs"
date:   2024-11-13 08:00:00 -0600
excerpt: "An introduction to the common design pattern."
---
# What are Cron Jobs?
To put it plainly, cron jobs are workflows that are executed on a specific schedule. It could be every hour or even every minute, but the idea is that some code "wakes up" after every X unit of time and performs some action.

Cron jobs are very common in production systems. 

## Examples
- **Emailing customers.** Perhaps you have a newsletter that you want to send to subscribers once per week. You could configure a cron job to:
    - wake up once per week, 
    - fetch all of the eligible users in the database, 
    - and send an email to each user.
- **Tracking packages.** What if your business regularly buys packages on the internet and you need a way to automatically get notifications when a package is delivered? You could configure a cron job to:
    - wake up once every 10 minutes,
    - find all tracking numbers which have not yet been delivered in a database,
    - query an external API with the tracking number to get tracking updates
    - send a text message using the Twilio API when the tracking updates shows as "delivered."

# Pros and Cons of Cron Jobs
Cron jobs are often simple to understand and set up. They are versatile and work for many different business use cases. Where cron jobs fall short is when real-time updates are necessary. Because cron jobs only execute every X amount of time, by using them we are introducing latency into our system. Let's put this in the context of one of our examples to explain what I mean.
- **Tracking packages.**
    - In this example, a cron job wakes up every 10 minutes. This means we are introducing 10 minutes of latency between tracking updates. In the worst case scenario, a package could have been delivered 9 minutes and 59 seconds ago, but the customer didn't get a notification until the job wakes up on the 10 minute mark. 

## Why can't we just run a cron job every minute then?
In some cases, this solution is fine. The truth is, it depends on the system and the workflow. In some cases, running a cron job every minute may overload the database or cause other operational issues. 

# Cron Job Alternatives
What is the alternative to cron jobs? Well, for some cases, a cron job truly is the simplest and most effective approach. Other systems may have more strict requirements though. In that case, we look to **Event Driven Programming.**

## Event Driven Programming
A couple of patterns that fall under the event-driven umbrella are:
- Message queues
    - using a message queue, one process posts a message, and another process "dequeues" that message and immediately does some processing
- Webhooks
    - instead of querying an API for tracking updates every 10 minutes, what if the external API could just send me a message over the network when there's a tracking update? That's what webhooks are for. 

# Key Takeaways
- Cron jobs are workflows that automatically re-run every X unit of time.
- Cron jobs are easy to use and conceptualize, but introduce latency into your system. 
- Event driven programming paradigms such as message queues and webhooks can be used as an alternative to cron jobs when low or real-time latency is desired. 





