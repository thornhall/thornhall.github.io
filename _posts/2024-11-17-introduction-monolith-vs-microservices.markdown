---
title:  "Design Patterns: Monoliths vs. Microservices"
date:   2024-11-17 08:00:00 -0600
excerpt: "What does each application type constitute?"
---
## Background
Most software system architectures fall into two categories: microservices or a monolith. In this document, I will define what these terms mean. Then, I will discuss tradeoffs between the architectures. We will begin by defining key ideas for understanding these architectures.

## What is an Application?
To understand monoliths vs. microservices, we need to first establish the idea of what an application is. In this case, we can consider an application to be any server running code (usually inside of a container). The application is typically provisioned with any infrastructure it needs, such as databases, caches, or queues.

## What is a Domain?
When discussing monoliths vs. microservices, it also helps to know what a domain is. A domain is essentially a problem space. A problem space is a set of similar problems being solved by a software application.
- Example: In Yelp, a Review could be one domain, and a Business could be another domain.
- Example: On TikTok, the For You Page could be one domain, and the TikTok Shop could be another domain. 

It's important to know that it's possible to slice the domains of a business in different ways, and picking the best "slices" of domains is a challenge in and of itself in software system design.

## What is a Monolith? What is a Microservice?
Now that we have basic definitions for domains and applications, we can define monoliths and microservices.

### Monolith
A monolith is a single application that solves all (or many) problem domains of a business. All of the application code is stored in the same codebase.

### Microservice
A microservice is a single application that solves a single problem domain. In this architecture, one business will consist of many microservices. Each microservice has its own codebase.

### Example Monolith vs. Microservice
<figure>
    <a href="/assets/images/monolith_vs_microservice.png"><img src="/assets/images/monolith_vs_microservice.png"></a>
    <figcaption>A diagram showing a microservices architecture (left), and a monolith architecture (right). For the microservices architecture, we can see three different problem domains (Users, Tweets, Messages) belong to three services. Each service has its own database. On the right is a monolith architecture - just one application contains all of the business logic for each problem domain. In this example, the database is shared as well.</figcaption>
</figure>

### What is Deployment?
Deployment is the process by which we replace old running code with new code. This is important to understand in 
the context of monoliths vs. microservices, because each architecture has implications for deployment. 

### Pros of a Monolith
- **Ease of development**. Many monolith applications utilize frameworks that significantly decrease the time it takes to make an MVP. Without the operational overhead of microservices, smaller teams can be more agile.
- **Centralized business logic**. With all of your business logic in one place, the cognitive complexity of managing the code might be lesser in the short term.

### Downsides of Monolith
- **Scaling issues**. Often, when we need to scale one part of a monolithic application, we have to scale the entire application (including unrelated components), wasting resources. 
- **Deployment issues**. When we need to deploy changes to one small part of the application, we have to re-deploy the entire application, even unrelated parts, which increases the probability of bugs.
- **Team velocity issues**. In a large organization using a monolith, many teams will be modifying the codebase at the same time and at varying cadences. Because all of the code is shared, deployment has to be carefully coordinated between teams, which can slow teams down. 
- **Increased codebase complexity**. A monolith codebase is often very complex and highly coupled. Changes in one area can cause unintended side effects in another. This increases the probability of bugs.

### How Microservices Help Solve these Challenges
- **Scaling**. Each microservice can be scaled independent of other domains, saving resources and allowing for more fine-tuned control and resource management.
- **Deployment**. Changes to one part of a domain do not affect the code of another domain. This means we can deploy changes with greater confidence and less risk. 
- **Team velocity**. In a microservices architecture, each team owns their own microservice. Because the code is not all in one codebase, each team can independently make changes to and deploy their own microservices on their own schedule without worrying about other teams' changes.
- **Decreased code complexity**. Microservices keep code modular and lean. Codebases can still get complex but they do so at a much slower rate than monoliths.

### Microservices Aren't Without Their Own Issues
- **Architectural Complexity**. A microservices architecture is much more complex than a monolith. Now, we have multiple different codebases to manage, each with their own dependencies and versions.
- **Operational complexity**. Each service has its own infrastructure for deployment, monitoring, and logging. Managing several different applications can be challenging. Kubernetes can help solve this issue. 
- **Network overhead**. Microservices need to communicate with each other. Because they are separate applications, they do so over the network which adds all of the complexities of networking. This means handling transient failures, slowness, and retries. This can also lead to increased latency.
- **Data consistency in distributed systems**. Because each service typically has its own database, it can be challenging to keep all data consistent across microservices.

### When to Use a Monolith
A monolith is ideal for smaller businesses that need to get to the market quickly. In the short term they allow teams to be more agile. They do not always scale in the long term. 

## Key Takeaways
- Monoliths and microservices are the two primary application design patterns.
- A monolith is a single application containing all of a business's business logic, while microservices are several different applications solving each domain independently. 
- Monoliths are often simpler to implement than microservices, but face their own technical and organizational scaling issues. 
- Microservices solve many problems caused by the monolith architecture, but introduce their own set of problems.




