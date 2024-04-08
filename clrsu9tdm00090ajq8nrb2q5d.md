---
title: "eBPF, Service Mesh and Sidecar"
seoTitle: "eBPF, Service Mesh and Sidecar"
seoDescription: "The operating system is like the boss of your computer, handling security, networking, and keeping an eye on what's happening."
datePublished: Wed Jan 24 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clrsu9tdm00090ajq8nrb2q5d
slug: ebpf-service-mesh-and-sidecar
canonical: https://keploy.io/blog/community/ebpf-service-mesh-and-sidecar
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705497247364/68117111-400b-4a29-b3dc-70627ff95c2b.png
tags: servicemesh, kubernetes-container, ebpf, sidecar

---

The operating system is like the boss of your computer, handling security, networking, and keeping an eye on what's happening. But tweaking or improving the core part of the operating system, called the kernel, is a bit tricky because it's mainly focused on keeping things stable and secure.

Most cool new stuff usually happens outside the core system, in what we call the user space. That's where people add extra features or functions. However, thanks to "[**eBPF**](http://ebpf.io)", we now have a way to make big improvements right at the core level of the operating system. This has opened the door for some really exciting changes in how our computers handle networking, security, and keeping track of what's going on.

There are many use cases of eBPF, such as:

* **High-Performance Networking for Cloud-Native Apps.**
    
* **More efficient authorization of network traffic at the third (L3) and fourth (L4) layers of the OSI model.**
    
* **Fine-Grained Observability Data Insights with Low Overhead.**
    
* **Insights for Troubleshooting Application Performance.**
    
* **Enforcing security policies at the kernel level**
    
* and more....
    

We have many frameworks that allows you to get started with eBPF programming, some of them are:

* **libbpf**: This is a C library that provides a portable and efficient way to work with eBPF programs. It is maintained by the Linux kernel community and is widely used in many eBPF applications.
    
* **bpftrace**: This is a a high-level tracing language for Linux eBPF that uses [LLVM compilers](https://en.wikipedia.org/wiki/LLVM) and BCC framework for interacting with the Linux eBPF subsystem.
    
* **Cilium/ebpf-go**: It is a pure Go library that provides utilities for loading, compiling, and debugging eBPF programs. It has minimal external dependencies and is intended to be used in long running processes.
    
* **BCC**: This is a collection of tools and libraries for working with eBPF programs. It includes a set of scripts and examples that demonstrate how to use eBPF for various use cases, such as network monitoring, performance analysis, and system profiling.
    
* **ebpf-tiny**: This is a lightweight eBPF framework that provides a simple and efficient way to develop and deploy eBPF programs. It is designed for embedded systems and resource-constrained environments.
    

## **What is Service Mesh?**

Many service mesh products aim to make it easier for microservices in applications to connect with each other. They offer benefits like secure connections, observability, and traffic management. However, the enthusiasm for service mesh is often dampened by worries about added complexity and overhead. Let's delve into how eBPF (extended Berkeley Packet Filter) can help simplify the service mesh. By leveraging eBPF, we can make the data plane of the service mesh more efficient and simpler to set up.

![servicemesh intro](https://isovalent.wpengine.com/wp-content/uploads/2022/05/servicemesh_intro-1024x341.png align="left")

## Sidecars & eBPF - Service Mesh Model

### **Sidecar proxy model**

The sidecar proxy model is the most common way of implementing service mesh today. Sidecars are like extra helpers for your pods, working right alongside them. They act as tiny traffic managers, dealing with the incoming and outgoing data of the pods. Think of them as traffic cops for your applications.

![eBPF-based model](https://isovalent.wpengine.com/wp-content/uploads/2022/05/comparison_sidecar-model-1024x273.png align="left")

These sidecars handle important tasks, like directing the flow of data, making sure things are evenly balanced, keeping information secure with encryption, and deciding who gets access. They also keep track of how much traffic is going in and out, gathering data about what's happening.

![Sidecar-based service mesh model](https://isovalent.wpengine.com/wp-content/uploads/2022/05/sidecar_model-1024x378.png align="left")

In simple terms, sidecars are like guardians for your pods, making sure everything runs smoothly and safely. However, the sidecar proxy model also has some disadvantages like:

* Increase resources whose consumption as the load to your pod increases, plus they add latency.
    
* The proxy may not always reflect the true state of the application or the network.
    
* Each additional component, like a sidecar proxy, increases the attack surface by providing more entry points for potential threats.
    
* Introduces additional points of interaction, and if not properly secured, these communication channels can become targets for attackers.
    

The sidecar models are slower because of buffer-copying and context switching into the user space; simply, any packet from one service to another has to traverse through the TCP/IP and socket stack multiple times. And this part can be eliminated completely by using eBPF.

### **eBPF based model**

Unlike sidecar method, where each container needs a small companion proxy container running alongside it in the same pod, eBPF operates at the host operating system's kernel level. This means that it runs only once on each host, regardless of the number of containers or pods scheduled to run on that host.

In the eBPF-based service mesh model, the system operates by attaching eBPF programs to different network events, essentially intercepting and managing the communication between services. For instance, a specific eBPF program can be linked to a "**socket connect() call"**, redirecting the traffic to a local port where another eBPF program is actively listening.

![eBPF Service Mesh Architecture](https://isovalent.wpengine.com/wp-content/uploads/2022/05/ebpf_servicemesh-1024x450.png align="left")

As a result, the application believes it's connecting to a remote service, but in reality, it's connecting to a local eBPF program responsible for managing the service mesh logic. This approach allows for effective control and processing of the communication flow between different services in the system.

![eBPF-based model](https://isovalent.wpengine.com/wp-content/uploads/2022/05/comparison_sidecar-free-model-1024x257.png align="left")

Additionally, Since the eBPF program's can communicate with other eBPF programs that are operating on separate nodes or pods. They can exchange some necessary information, including **data related to service discovery**, **configuration details**, policies, and more. This inter-program communication enables coordination and information sharing across the distributed environment, contributing to the effective functioning of the eBPF-based service mesh.

### Can eBPF and sidecar be used together?

In short answer, "yes", when and how?

Choosing between eBPF and the sidecar model depends on the specific requirements and priorities of the system architecture. *For examples,* in scenarios where low-level network optimization and efficiency are important, there eBPF may be the preferred choice. Vice-a-versa, when the focus is on managing application-level concerns, the sidecar approach may offer a more practical solution. It's not necessarily an "either-or" situation; in some cases, a combination of both approaches might be employed to leverage their respective strengths in different parts of the system.

## Conclusion

There's no universal solution for service mesh architecture, and the choice between eBPF and the sidecar model mostly depends on the specific requirements and characteristics of the given use case and environment. Combining both models, is a pragmatic approach that allows leveraging the strengths of each in different layers of the system.

Using eBPF for L4 (transport layer) traffic management and security addresses lower-level network concerns efficiently. Meanwhile, employing a sidecar proxy for L7 (application layer) traffic management and observability enhances the handling of application-specific logic and monitoring.

This hybrid approach acknowledges the diversity of challenges within a service mesh and tailors the solution accordingly. It's a flexible strategy that recognizes the nuances of different layers in the networking stack and adapts the technology stack to provide optimal solutions for each layer's unique demands.

## FAQ's

### **What is Cilium?**

Cilium is a cloud native technology for networking, observability, and security. It is based on the kernel technology eBPF, originally for better networking performance, and now leverages many additional features for different use cases.

### What are the benefits of using a service mesh in microservices architectures?

Using a service mesh in microservices architectures offers several benefits.

* Firstly, it provides centralized control over communication between services, enabling features like traffic routing, load balancing, and fault tolerance without requiring changes to application code.
    
* Secondly, it improves observability by facilitating metrics collection, distributed tracing, and logging, allowing for better insight into service interactions and performance.
    
* Thirdly, a service mesh enhances security by enabling encryption, authentication, and access control policies to be applied uniformly across all services.
    
* Additionally, it promotes scalability by offloading common networking concerns to a dedicated infrastructure layer, reducing the complexity of individual services.
    

### What is eBPF and how does it work in modern networking?

In a simpler terms, a eBPF is a kernel technology that lets programs run without needing to add additional modules or modify the kernel source code.

eBPF allows for dynamically processing of network packets within the Linux kernel. Thus, enables developers to run custom programs safely and efficiently within the kernel, as well as providing them with deep visibility and control over network traffic. eBPF programs can be attached to various hooks throughout the kernel, allowing them to intercept and analyze packets at different stages of their journey through the network stack.

### What are risks associated with the eBPF ?

Basically, the eBPF is not a machine code, rather they are bytecode in the kernel. The VM sandboxes them and enforces access controls so only privileged users can run eBPF programs. Therefore all the network interaction that are happening can be either be captured, or malicious code call be injected and shared over the net later. You can also perform static analysis of the eBPF program to ensure that the eBPF programs are not harmful and that no information leaks from the kernel to the user space. 

***Resources & References: -***

1. [Home - Aya (aya-rs.dev)](https://aya-rs.dev/)
    
2. [Cilium Service Mesh - Everything You Need to Know (isovalent.com)](https://isovalent.com/blog/post/cilium-service-mesh/)
    
3. [How eBPF will solve Service Mesh - Goodbye Sidecars - Isovalent](https://isovalent.com/blog/post/2021-12-08-ebpf-servicemesh/)
    
4. [eBPF for Service Mesh? Yes, But Envoy is Here to Stay (solo.io)](https://www.solo.io/blog/ebpf-for-service-mesh/)
    
5. [eBPF Applications Landscape](https://ebpf.io/applications/)