---
title: "Blog 3"
date: 2026-07-07
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Modernizing Applications and Databases with GenAI on AWS

In the software development process, many businesses are still operating legacy systems based on a monolithic architecture. In the early stages, this model is usually easy to deploy, simple to manage, and suitable for small-scale systems. However, as the system grows, the number of users increases, and business requirements change continuously, monolithic architecture gradually exposes many limitations such as slow release cycles, difficulty scaling individual components, high operating costs, and the risk of cascading failures when an issue occurs.
Therefore, application modernization is not only about changing technologies or upgrading source code. It is also a process of changing the way we think about system design, operation, and software development.
This blog is summarized based on content from the Otokoshi team and focuses on the **GenAI-powered App & Database Modernization** solution. Instead of simply “upgrading code,” modernization is a combination of **Domain-Driven Design (DDD)**, **microservices**, **event-driven architecture**, **serverless computing**, and AI tools such as **Amazon Q Developer** or **AWS Transform**. These tools can support analysis, transformation, and optimization from the architecture layer to the source code and database.

## Architecture Guidance

A key change in the modernization process is moving from a large monolithic block to an ecosystem of smaller services, known as microservices. These services are designed to operate independently and are loosely connected through event flows following an event-driven model.
Instead of rewriting the entire system from scratch, a safer approach is to gradually extract each part based on business boundaries. This helps businesses reduce risks during the transformation process while gradually moving suitable components to serverless or microservices models.

**The modernization solution architecture can be illustrated as follows:**

![Figure 1. Architectural transition from Monolithic to Microservices combined with Event-driven and Serverless](images/3-BlogsTranslated/Blog3/blog3(0).jpg)

> *Figure 1. Architectural transition from Monolithic to Microservices combined with Event-driven and Serverless.*

When defining boundaries for components in the new system, the system usually has the following characteristics:
* Start with the business domain before choosing implementation technologies.
* Each important function is separated into an independent service that can operate on its own.
* Services communicate flexibly and asynchronously through events instead of relying completely on direct API calls.
* Source code can be deployed and operated without directly managing physical servers or virtual machines underneath.
When building a strategy to extract a monolithic system, several important factors should be considered:
* **Business**: Identify the main domains of the system, such as order management, payment, customer management, or inventory.
* **Architecture**: Use Bounded Context to divide the system into smaller parts, then decide which parts should remain and which should be separated into microservices.
* **Roadmap**: Prioritize modernizing the system step by step instead of transforming the entire system at once, in order to reduce risks and control progress more easily.

## Technology Selection and Compute Model

| System Aspect / Level of Control | Suitable AWS Services / Compute Model |
| --- | --- |
| Requires strict control over the operating system, runtime, and configuration | Amazon Elastic Compute Cloud (Amazon EC2) |
| Containerized applications, microservices, and orchestration requirements | Amazon Elastic Container Service (ECS), Amazon EKS |
| Running containers without managing the underlying servers | AWS Fargate |
| Event-driven workloads, short-running tasks, small APIs, automation | AWS Lambda |

## Domain-Driven Design (DDD)

![5-step application modernization process](images/3-BlogsTranslated/Blog3/blog3(1).jpg)

A common mistake when modernizing a system is starting with the question: “Should we use Lambda, ECS, or Kubernetes?” However, the more important question that should be asked first is: “What business purpose is this system serving?”
**Domain-Driven Design (DDD)** helps technical teams and business teams communicate using a shared language. Through *event storming* sessions, the development team can clearly identify:
* Important business events and the actors involved.
* The timeline of the main processing steps in the system.
* The bounded contexts that need to be extracted to support the modernization process.
However, DDD also has certain challenges. This approach requires continuous collaboration between *domain experts* and *software experts*. Both sides need to analyze, discuss, and clarify the complexity of the business domain before starting the coding phase.

## Event-Driven Architecture

Event-Driven Architecture helps solve the communication problem between components in a system. If microservices continuously call each other using synchronous APIs, the system can easily fall into tight coupling. When one service fails or is interrupted, other services may also be affected.
With the event-driven model, the processing approach becomes more flexible:
* A service, such as the Order Service, only needs to publish an **OrderCreated** event.
* Other services such as Payment, Inventory, or Notification independently listen to that event.
* Event coordination can be handled through services such as **Amazon EventBridge**, **Amazon SNS**, or **Amazon SQS**.

> This approach makes the system more flexible, easier to scale, and reduces direct dependencies between components. As a result, the overall architecture becomes more loosely coupled and more stable when operating at scale.

## Compute Evolution: Moving Toward Serverless

An important direction in application modernization is gradually moving toward a serverless model. With this model, development teams do not need to spend too much time managing the underlying infrastructure.
Tasks such as server maintenance, scaling, patching, and capacity management are handled by AWS. As a result, developers can focus more on business logic and the speed of feature development.
A typical service in this approach is **AWS Lambda**:
1. It is suitable for event-driven workloads.
2. It optimizes costs through automatic scaling and charges only based on actual usage.
3. It allows developers to focus on business logic instead of server management.

## The Role of GenAI in the Solution

### 1. Amazon Q Developer and AWS Transform

GenAI cannot completely replace the role of a software architect. However, in the system modernization process, GenAI can act as a powerful technical assistant that helps shorten the time needed for analysis, transformation, and source code optimization.
![GenAI support through Amazon Q Developer and AWS Transform](images/3-BlogsTranslated/Blog3/blog3(2).jpg)
* **Amazon Q Developer**: Supports legacy codebase analysis, suggests ways to extract modules, generates technical documentation, proposes test cases, and assists with refactoring. For example, this tool can help upgrade Java versions, explain dependencies, or suggest improvements to the source code structure.
* **AWS Transform**: An agentic AI service that supports large-scale modernization, especially for complex workloads such as Windows, mainframe, or VMware.

The example below illustrates a diff-style transformation plan suggested by AI. The goal is to replace synchronous API calls in the legacy system with an event publishing model through EventBridge:

```diff
- // Legacy Monolithic Synchronous Call
- paymentService.processPayment(order.getId(), order.getTotal());
- inventoryService.deductItems(order.getItems());

+ // Modern Event-Driven Architecture with EventBridge
+ PutEventsRequestEntry eventEntry = PutEventsRequestEntry.builder()
+    .source("com.ecommerce.orders")
+    .detailType("OrderCreated")
+    .detail(orderJson)
+    .eventBusName("EcommerceEventBus")
+    .build();
+ eventBridgeClient.putEvents(PutEventsRequest.builder().entries(eventEntry).build());
```