---
title: Scaling out endpoints
summary: Scenarios when to scale out your endpoints and information on how bus and broker transports behave in scale-out scenarios
reviewed: 2018-09-28
component: Core
tags:
- scalability
- Routing
redirects:
- nservicebus/scale-out
---

## When to scale out

### Throughput

Scaling out is useful when the work being done by a single machine takes time and therefore more computing resources are necessary. To help with this, monitor the [CriticalTime performance counter](/monitoring/metrics/performance-counters.md) on the endpoint.

### High-availability

Some scale-out techniques improve the availability characteristic as well as throughput. When scaling out for higher availability, keep in mind the components that are single points of failure. Two examples are the distributor when using MSMQ or the message broker for broker transports. In these scenarios, use appropriate risk mitigation techniques (e.g. use failover cluster to host the distributor or cluster mode if a given broker supports one).  

## Bus transports

In bus transports like MSMQ, there is no central place from which multiple instances of an endpoint can receive messages concurrently. Each instance has its own queue so scaling out requires distributing messages between the queues. The process of distributing messages can be performed either on the sender or by a specialized proxy. Refer to the [MSMQ documentation](/transports/msmq/scaling-out.md) for more details regarding this particular transport.


## Broker transports

The main difference when using broker transports (SQL Server, RabbitMQ, Azure Service Bus, Azure Storage Queues, and Amazon SQS) is that queues are not attached to machines but rather are maintained by a central server (or cluster of servers). This design enables usage of the competing consumers pattern for scaling out processing power. All instances connect to the same queue.

partial: concurrency