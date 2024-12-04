---
title:  "Design Components: Message Brokers"
date:   2024-12-04 08:00:00 -0600
excerpt: "What are message brokers?"
---
## Background
In my discussion of [queues](https://thornhall.github.io/tech/2024/11/30/queues.html), I introduced the concept of a queue and what types of benefits it brings to the table. I encourage the reader to read that article before this one. 

**Message brokers** are components that are built on top of a queue. They offer more functionality than simple queues. 

## Functionalities
A message broker is software that enables and simplifies the communication between different programs. The programs may be written in any language; the message brokers are language agnostic. Most languages have libraries that simplify the usage of common message brokers. 

Here are some key benefits provided by message brokers:
- **Message delivery assurances**. While a simple queue does not guarantee the delivery of a message, a message broker maintains state that tracks the delivery of a message and ensures the message is processed at least once. It handles message delivery retries as well.
- **Multiple consumers**. Message brokers allow for multiple consumers to subscribe to a particular message queue. In the simple queue example, only one process could consume the queue.
- **Data durability**. Some message brokers like Kafka allow for the state of the queue to be periodically flushed to hard disk, increasing data durability. 
- **Protocol translation**. Brokers can often translate between different communication protocols, making them especially useful for communicating between different processes using different protocols. 

## Example: Multiple Consumers
<figure>
    <a href="/assets/images/message_broker_example.png"><img src="/assets/images/message_broker_example.png"></a>
    <figcaption>In this example, we have a sports stats application that periodically fetches stat updates from an external API. When an event happens (such as a player scoring), multiple consumers are notified. A consumer for giving users mobile updates consumes the update and sends a mobile notification. A consumer related to sports bets updates odds and other data related to betting. Finally, a fantasy leagues consumer updates fantasy-league-related data. All consumers are consuming the same messages thanks to message brokers.</figcaption>
</figure>

## Popular Message Brokers
- Apache Kafka
- RabbitMQ
- Amazon SQS

## Key Takeaways
- Message brokers are built on top of queues and enhance their functionality. 
- Message brokers allow for multiple consumers to read from one queue.




