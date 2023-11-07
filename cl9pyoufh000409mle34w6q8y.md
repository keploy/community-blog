---
title: "Teleport into tech space through API Gateways"
seoTitle: "Teleport into tech space through API Gateways"
seoDescription: "The blog covers the architectures used in building software and API gateways acting as an abstraction layer to connect microservices with client-side apps."
datePublished: Thu Aug 11 2022 18:16:20 GMT+0000 (Coordinated Universal Time)
cuid: cl9pyoufh000409mle34w6q8y
slug: teleport-into-tech-space-through-api-gateways
canonical: https://medium.com/@sanskritiharmukh/teleport-into-tech-space-through-api-gateways-278eca0c7fe4
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1660157888480/EjoQDWQZH.png
tags: microservices, apis, api-gateway, osi, monoliths

---


You might be wondering, Is there a need to use an API Gateway? Can API alone cannot handle all the communication between the users and services? Well, Let’s tackle all the questions coming to your mind one after the other. So, are you ready for a ride into the world of APIs?

Before Diving deep into API Gateway and its working, we will understand the monolithic and microservices architecture.

A hot architecture that we use today for developing software applications is Microservice. But what brought this evolution? What was the old mechanism that we used to carry the development on? It’s time to press the rewind button and get to know the old Monolithic architecture.


### **Monolithic Architecture:**

Monolithic in general means a single large entity, whereas in IT, it can be thought of as a package that contains all the sub-services and parts of the application, deployed as one unit.

If you are familiar with the famous OSI model of Computer Networking, you know the working of layers present in Monolithic Architecture. If not Let’s learn quickly what each tier represents.

- *Presentation Layer: It is often viewed as the Frontend layer of a three-tier architecture. It is built using web technologies like HTML, CSS, JS, React, and other web-based 8frameworks. To connect with the rest two layers, it makes API calls.*

- *Application Layer: The business logic of the application is built here, mostly based on languages like C++, Java, C#, Python, .NET, and many more.*

- *Data Layer: A layer where all the data is stored and accessed by the Application tier via API calls. To organize the data, DBMS like MySQL, Oracle DB, and MongoDB is used.*


![monoarch.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659980767221/6UBHzNUnI.png align="center")


> To visualize this concept better, let’s consider an example, Think of a social networking application with business services such as Chat System, User Interface, and Notifications settings tightly coupled and packaged using JAR(Java Archive). This unit is now hosted over the TOMCAT server. Now, this is where things mess up. As all the services are deployed on the same server thus, If I want to perform even the slightest change in just one service, I’ll have to redeploy the entire application to function efficiently. Scalability and Flexibility are the major areas of concern with Monolith.


![mono.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659980617149/cs0QZjInW.png align="center")

### **Time for evolution: Microservices**
Let’s divide the Buzz word. Micro means small, and Services indicates the functionality for clients or application. Combining these two, we get Microservices that signify a multi-component service built to satisfy a specific concern of the application. Contrary to Monolith, The sub-services of application in this architecture are deployed at separate servers, work independently, and communicate with one another using well-defined APIs.

> For example, the User Interface is written in Java, Chat System in NodeJS, and Notification settings in Python. They have their APIs designed to interact, and all of them are deployed on a respective server thus, making a change in any one of the sub-services will be more efficient to carry. But what if I have hundreds of Microservices with APIs, calling each one of them would be frustrating for anyone. Do we have a solution? Well, Of course. Here comes into the picture the hero of the show- API Gateway.

![host.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659981075869/42cSuYB9I.png align="center")


### **API Gateway**
The UI/UX person integrating your social networking application does need not to know about all the microservices working at the Backend. Dealing with hundreds of them is not an easy task. So, to resolve this problem, an abstraction layer is supposed to be created. This layer will be routing the requests made outside the microservices and will respond to the target. The only information now that you as a backend developer have to give to the frontend web developer is about the Abstraction Layer. This layer is known as API Gateway as it defines a path for each request or also called Edge Microservice, as it sits at the edge of Microservice architectural diagrams.


![micro.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659981103333/v7uFPIYzg.png align="center")

**How to introduce an API Gateway?**

*Step 1: Prepare an External API Contract*

*As you’ll not be comfortable letting everyone from the outside world access the APIs of your application, therefore, design a list of APIs associated with microservices that you are okay with anyone calling.*

*Step 2: Figure out the Abstraction Layer*

*Create or use the existing microservice that you have decided to make public to act as a bridge satisfying the requests. This technique is often referred to as API Composition. One can refer to already existing API gateways such as Zuul by Netflix, Kong Gateway by NGINX, Apache APISIX, and the list goes on.*

As there will be a lot of requests an API Gateway will be handling, thus considering just one API Gateway is not recommended as it increases the load on it, slowing down the entire system. Multiple Gateways divide the task and using load balancers before API Gateways is helpful to segregate the calls.

Load Balancers and API Gateway are often considered on the same level, as their functionalities are similar but their way of managing the requests are different. API Gateways directs the calls to specific resources, unlike Load Balancers which distribute the requests equally to all backend services.

When we have a variety of clients with different sets of demands for API and configuration, a generalized API gateway as a single entry point may not work. In such a case, we can have API Gateways dedicated to the specific type of client like Web API Gateway for Web Application, Mobile API for Android and iOS Applications, and for third-party applications, a Public API Gateway. This pattern is popularly known as “Backend for Frontend”.


![Gateways.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1659981118446/wVWoBMySO.png align="center")

Yay🎉! You have reached the end of this blog. I hope I was able to explain to you the following:
- Monolith and Microservices
- What is API Gateway?
- Where is it used?
- The variations of API Gateway.

Thank you for sticking till the last. I am glad to be a part of your learning journey🤍.

