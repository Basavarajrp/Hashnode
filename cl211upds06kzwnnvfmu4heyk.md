---
title: "Role of RabbitMQ in Microservices Architecture"
seoDescription: "RabbitMQ in Microservices Architecture"
datePublished: Fri Apr 15 2022 23:16:29 GMT+0000 (Coordinated Universal Time)
cuid: cl211upds06kzwnnvfmu4heyk
slug: role-of-rabbitmq-in-microservices-architecture
canonical: https://basavarajrp.hashnode.dev/role-of-rabbitmq-in-microservices-architecture
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650056775063/7LyA3bwuU.png
tags: microservices, programming-blogs, python, message-queue, rabbitmq

---

## Hey There 👋
Hey folks, I hope everything is going well for you. I came up with an interesting topic to discuss with you all.

It's been a year since I started my journey as a software developer and have been working with microservices architecture and message queues and I thought to share some of the things I know related to these topics.

Before starting a discussion grab your favorite drinks🥤. I will just try my best to understand you all about what is RabbitMQ and when to use them generally, I will not go in-depth into the technical part now but in a future post, I will go in-depth technically.

Let's go 🚀🚀...

### What is RabbitMQ?
![RabbitMQ (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1650056878204/w_nthD9GO.png)

When you visit a restaurant you place an order, and the waiter takes your order and forward the order to the kitchen department, now the waiter will not wait until your order is fulfilled they go and take other orders and again forward the order to the kitchen department, once your order is ready waiter will serve your favorite food. Here in this scenario kitchen department and the waiter has their own responsibility and they don't depend on each other to complete the task and neither is constrained by time.

In the above example, the customer(Producer) publishes the order to the waiter(Message broker), and the waiter forwards it to the kitchen service(Consumer).

RabbitMQ is a message broker which receives messages/requests from one service(Producer) and forwards that to another service (Consumer) to process the request. It is used for software applications to connect and scale, and also helps in decoupling applications into individual services and connecting them.

In microservices architecture communication between different microservices can be Synchronous or Asynchronous. In the Asynchronous method, we have a central hub where the request from one service to other services takes place. This is where RabbitMQ is used.

In the above image the two services, one is the producer and the other one is consumer are communicating with each other with RabbitMQ as a central hub, the producer and consumer are independent of each other.

### When to use RabbitMQ?

![client-server (3).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1650061148825/361wDlcrw.png)

Consider the client-server architecture when a client makes a request to the server the client will be waiting for the response back, here the server is forced to perform the task immediately since the client is waiting for the response, the server may take a long time to complete the task this kind of dependency affects the performance of the application and also this not user friendly.

If the server goes down the client has to retry potentially multiple times in order to get a successive response, from this we can tell that client is heavily dependent or tightly coupled to the server. 

In this kind of situation, we can go with Horizontal scaling by adding multiple servers and placing the NGINX as a reverse proxy and configuring it as a loader balancer for upcoming requests. But as we know Horizontal scaling is resource hungry, so people go with microservices architecture with message Queues for performing the messages/requests asynchronously, RabbitMQ resolves this dependency and scaling issues in the applications and allows the clients and server to perform operations independently with less overload.

If there is a condition to run some program or process in the background that can be delivered for a later process then Rabbit MQ can be used as the message broker to that service.

### What happens to messages when the consumer is down ❌.
![consumer down.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1650058903334/B1ux0F871.png)

Think you have Microservice architecture and one service is communicating to the other service, and something goes wrong for the consumer service and it went down for some time but the producer service keeps on sending more messages/requests still all these messages/requests will be available in the message queue, and when service recovers from the failure all the request/messages are forwarded to that respective consumer service.

Most of the time in the initial days the applications follow the monolithic architecture where the entire application is built using a single codebase to prove their business model to investors/customers. Once the application gets more customers and high traffic they move from a monolithic architecture to microservices.

In a microservices architecture, the entire application is divided into small microservices. Consider the E-Commerce application, in these application payment services, maybe one service and the product orders can be another separate service. When we have a microservices architecture each service can communicate to each other asynchronously, and RabbitMQ acts as the message broker or central hub between two services for the exchange of requests/messages.

A service publishing a message does not need to know anything about the inner workings of the services that will process that message. This way of handling messages decouples the producer from the consumer. The producer service just publishes the messages/requests to messaging broker without worrying about the consumer service state, like this all services can work independently and asynchronously without worrying about others completing the task.

That's all for this article, If you are reading this then you reached the end, I hope you got to know something about RabbitMQ and its use cases. In the future article, we will go into more technical aspects of RabbitMQ and its implementation.

Thank you.

### References:
[Hussein Nasser](https://youtu.be/W4_aGb_MOls) 
