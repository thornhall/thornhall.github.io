---
title:  "Design Components: Load Balancers"
date:   2024-11-26 08:00:00 -0600
excerpt: "An intro to load balancers."
---
## Background
In our discussion of [horizontal scaling](https://thornhall.github.io/tech/2024/11/17/intoduction-horizontal-scaling.html), we found out that modern applications utilize horizontal scaling to handle increased load. However, horizontal scaling would be meaningless if we didn't have a way to distribute traffic evenly to each server instance. This is where load balancers come in. 

## What is a Load Balancer?
A load balancer takes API requests and distributes them evenly to a horizontally scaled application. It's necessary because otherwise all of our traffic would be randomly going to each server instance, meaning one could be easily overloaded. 

Load balancers distribute the traffic in a pattern that avoids overwhelming any one server instance.

<figure>
    <a href="/assets/images/load_balancer.png"><img src="/assets/images/load_balancer.png"></a>
    <figcaption>A diagram depicting a load balancer's functionality. From left to right, we can see 8 requests come in. The 8 requests are routed to the load balancer, which then handles routing 2 requests to each of the 4 server instances to keep the load distributed evenly across all servers. </figcaption>
</figure>

## Load Balancing Strategies
Load balancers may employ different strategies for distributing requests. The best strategy is dependent upon your use case. The most common of the distribution strategies is Round Robin. 

Here are some of the common strategies:

1. Round Robin
- **How It Works**: Requests are distributed sequentially to each server in the pool, looping back to the first once the end of the list is reached.
- **Use Case**: Best for evenly distributed workloads where all servers have similar capacity and performance.

2. Weighted Round Robin
- **How It Works**: Similar to Round Robin, but each server is assigned a weight based on its capacity. Higher-weighted servers receive more traffic.
- **Use Case**: Useful when servers have different performance characteristics.

3. Least Connections
- **How It Works**: Routes traffic to the server with the fewest active connections.
- **Use Case**: Best for scenarios where connections have varying durations (e.g., streaming, long-lived sessions).

4. Least Response Time
- **How It Works**: Routes requests to the server with the fastest response time and fewest active connections.
- **Use Case**: Ideal for applications where latency is critical.

5. IP Hash
- **How It Works**: Uses a hash function on the client’s IP address to determine the server to route traffic to.
- **Use Case**: Useful for sticky sessions where the same client must connect to the same server.

6. Geographic Load Balancing (Geo-Based)
- **How It Works**: Routes requests to servers based on the client’s geographic location.
- **Use Case**: Reduces latency by directing users to the nearest server. Useful for global applications.

7. Weighted Random
- **How It Works**: Randomly selects a server for each request, with higher-weighted servers having a greater probability of selection.
- **Use Case**: Suitable for load distribution where randomness helps balance uneven traffic spikes.

## Key Takeaways
- Load balancers are used with horizontally-scaled applications to distribute load across them evenly.
- There are different strategies for load balancing. The one you use is dependent upon your use case. 