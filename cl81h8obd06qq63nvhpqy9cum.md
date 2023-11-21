---
title: "Types of APIs and API architecture"
datePublished: Wed Sep 14 2022 10:25:29 GMT+0000 (Coordinated Universal Time)
cuid: cl81h8obd06qq63nvhpqy9cum
slug: types-of-apis-and-api-architecture
canonical: https://keploy.io/blog/community/types-of-apis-and-api-architecture
tags: apis, keploy, keploy-api-fellowship

---



### What is an API ? ###
****Application programming interfaces, or APIs as they are more generally known, allow organizations to link systems, apps, devices, and datasets and to unlock data. It's important to understand which kind of APIs will be most effective for a project based on its intended use case, the people who will use and access these APIs, and the systems and datasets that must be linked. Determining the best kind of API to construct and designing the architecture accordingly are essential for efficient API performance and API maintenance. ****

# What is an API in general understanding?

API acts as a link between the frontend and the backend. For example, if you went to the hotel and called the waiter to take your order, the waiter would go to the kitchen and tell the chef that this table number wants to eat these dishes. In technical terms, the frontend is your hotel, the waiter is an API, and the kitchen is the backend. What the waiter did was take the order, which means receiving the request from the client (user) and sending it to the kitchen (backend). thats the task of an API.

# Different types of API’s

The API defines endpoints and suitable query and response forms. Web APIs are the APIs used for communication between browsers. Services such as site notifications and Web storage are available. Different online APIs, including free, corporate, and partner APIs, offer varying levels of protection and privacy. A composite API, which is a collection of data or service APIs, can be integrated with other web APIs.

1.Internal APIs:

It is also known as private, internal APIs that are hidden from others and used within a company. This method enables firms to simplify information exchange across departments and locations. Internal APIs, while internally accessible, nonetheless provide security mechanisms to validate the employee's identification before providing system access.

2.External APIs:

This API is also known as public or external, with open APIs providing relaxed security and allowing programmers and other users to freely access data. Some systems are completely transparent, while others may require a basic registry or API key. Because of this functionality, public APIs are an excellent alternative for businesses looking to expedite interactions with third-party users such as suppliers or customers. It also allows software developers to quickly and easily implement components.

3.Partner APIs:

Partner APIs, like the open approach, seek to facilitate collaboration between a company and its external users. More encryption is used. however, to enable data access to certain business partners. While partner APIs are commonly available to other public APIs, third-party gateways restrict access to registered services.

4.Composite APIs:

The composite API design can withstand several integration systems and all data. Because of this additional capabilities, Composite APIs are ideal for completing a single action with many services. It also gives developers access to numerous endpoints, such as web and other API applications, using a single function call. The strong composite API infrastructure improves data services and provides a system-integrated solution.

## Types of APIs by purpose

It’s rare that an organization decides it needs an API out of the blue — most often, organizations start with an idea, application, innovation, or use case that requires connectivity to other systems or datasets.

- **System APIs**: System APIs unlock data from core systems of record within an organization. Examples of critical systems that APIs could unlock data from include ERP, customer and billing systems, and proprietary databases. 

- **Process APIs: **Process APIs interact with and shape data within a single system or across systems — breaking down data silos. Process APIs provide a means of combining data and orchestrating multiple System APIs for a specific business purpose. Examples of this include creating a 360-degree view of the customer, order fulfillment, and shipment status. 

- **Experience APIs:** Experience APIs provide a business context for the data and processes that were unlocked and established with System and Process APIs. Experience APIs expose the data to be consumed by its intended audience — such as mobile applications, internal portals for customer data, etc.

 
## Types of API architectural styles

Another area of choice for an API is which architectural style or styles will be employed. If certain functional capabilities are required, it is critical to select an architectural style or pattern that best supports the intended use of the API. This is often an API design decision made by more technically savvy teams.


There are various styles of architecture for APIs, as well as varying data formats within these styles, below we’ve listed some of the most common:

- **REST: ** REST (Representational State Transfer) is an architectural style that separates the concerns of the API consumer from the API provider by relying on commands that are built-into the underlying networking protocol. Clients use the included links and forms to perform actions (e.g. read, update, share, approve. etc.). HTML is the best known example of this style and there are several other formats just for APIs (HAL, CollectionJSON, Siren, etc.). There are many benefits of REST APIs — including flexibility, and ability to accommodate popular data formats like JSON and XML among others.

- **RPC: ** Remote Procedure Calls — or RPCs — typically require developers to execute specific blocks of code on another system. RPC-style remote invocation of procedures other systems usually requires developers to call those procedures by name. RPC is protocol-agnostic, which means it has the potential to be supported on many protocols, but also loses the benefits of using native protocol capabilities (e.g. caching). The proliferation of non-standard procedure names from one RPC API to the next results in tighter coupling between API consumers and providers which in turn overburdens developers involved in all aspects of an RPC-driven API ecosystem. RPC architectural patterns can be observed in popular API technologies such as SOAP, GraphQL, and gRPC.

- **Event-driven/Streaming:** Sometimes referred to as evented, real-time, streaming, asynchronous, or push architectures, event-driven APIs don’t wait for an API consumer to call them before delivering a response. Instead, a response is triggered by the occurrence of an event. These services expose events to which clients can subscribe to receive updates when values on the service change. There are a handful of variations for this style including (among others) reactive, publish-and-subscribe, event notification, and CQRS.