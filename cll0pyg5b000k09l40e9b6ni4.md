---
title: "Stubs | Mocks | Fakes: Let's define the boundaries!!"
seoTitle: "Stubs, Mocks, Fakes: Let's define the boundaries!!"
seoDescription: "Learn the key differences between stubs, mocks, and fakes in software testing. Understand their roles and how to use them effectively for better test code."
datePublished: Fri Aug 04 2023 10:17:37 GMT+0000 (Coordinated Universal Time)
cuid: cll0pyg5b000k09l40e9b6ni4
slug: stubs-mocks-fakes-lets-define-the-boundaries
canonical: https://keploy.io/blog/posts/stubs-mocks-fakes-lets-define-the-boundaries
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690522769373/bc39f114-94c5-47b2-9b2d-d897eccadfb7.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1690522951950/94829c8e-d278-40eb-b604-f4e598de8079.png
tags: testing, mocks, test-doubles, stubs, fakes

---

While writing tests for an application, we may fall into different scenarios, which must be considered for writing tests, that are strong enough to check the robustness of the application.

Let it be Unit testing, Integration testing, or some other, we may encounter some situations where we need to isolate our implementation from dependencies or verify how the different components of the application interact with each other or use some alternate implementation of any element if the original is not feasible to be used while testing.

In these situations, it's crucial to identify which of these three i.e. Stubs, Mocks, or Fakes must be brought into use while testing.

So, let's consider some scenarios and understand how a tester must write tests in these situations.

### Stubs: An Isolating Agent!

Consider you're building an Online Education Platform that has a feature through which the students can upload their assignments in a PDF format. While uploading it, the file goes for processing first and then it's uploaded to the cloud.

![PDF Uploading Process](https://cdn.hashnode.com/res/hashnode/image/upload/v1690339675090/a4740d53-0abe-40cd-a5af-aca3eb22ed50.png align="center")

Now say, you want to unit test the File Processing module but it has the dependency of the File Uploading service, and every time a test is executed, the processed file will be uploaded to the cloud and this won't let the testing of the File Processing module achieve isolation and it then can't be tested solely.

This is where [Stubs](https://learn.microsoft.com/en-us/visualstudio/test/using-stubs-to-isolate-parts-of-your-application-from-each-other-for-unit-testing?view=vs-2022&tabs=csharp) come into play, to isolate and solely test the File Processing module, its dependency i.e. the File Uploading service will be replaced by a File Uploading Stub

![Stubs comes to play](https://cdn.hashnode.com/res/hashnode/image/upload/v1690371197861/3271050b-a799-4331-b886-504dea7b418b.png align="center")

The functioning of this stub will be in such a way that whenever it receives any processed file, it just returns a required Upload Success/Failure response accordingly without doing any computation or processing and also without uploading the file into the cloud.

It just returns the hardcoded response so that the expected behaviour of the File Processing Module can be compared with its actual behaviour. And in this way, the stub helps in achieving isolation. And generally, it's easy to create a stub and it's used to simulate a real-world object.

### Mocks: A Spy Verifying Elements Interact!

To understand [mocks](https://en.wikipedia.org/wiki/Mock_object), let's carry on with the same example of the Online Education Platform that has a User Registration module through which users can register themselves into the platform and access the content present in it. Upon registration, a Registration Success Email is sent to the users via an Email Sending Service.

![User Registration & Confirmation Process](https://cdn.hashnode.com/res/hashnode/image/upload/v1690373217756/859171a9-c79a-4270-9242-0775ac1f5570.png align="center")

Now say, you wanted to test:

1. Whether these two are integrated properly.
    
2. The number of times they interact with each other.
    
3. The User Registration module calls the Email Sending service with the correct parameters.
    

In this case, a mock must be brought into use. A mock is written to verify the interaction between modules and services with a set expectation value that corresponds to the number of times the modules and services must interact with each other while a test is running.

If the expected value is met, the test is passed, or else it fails. This means that in any particular call, the modules didn't interact with each other. And in this case, an Email Service Mock will be created to test the interaction.

![User Registration](https://cdn.hashnode.com/res/hashnode/image/upload/v1690377082625/276e1e73-1fa6-4234-88d3-fbef7b198a86.png align="center")

This Email Service Mock doesn't send an email, but it only tests and verifies the code behaviour and performs its sole purpose. It may or may not always be hard to create a mock, and it depends upon the scenario of what the application is and what the services & modules are included in it.

### Fakes: Budget-Friendly Replicas, Quality Uncompromised!

Considering a situation in which your Online Education Platform has some Premium Content that can only be accessed by a user having a premium subscription and if a regular user tries to access it, they're redirected to the Payment Gateway to purchase the subscription and then access the content, and if they make the payment, they've the access to see it.

![Fake Connection](https://cdn.hashnode.com/res/hashnode/image/upload/v1690378351922/c80e5af3-38ac-4eb6-94e1-827417b10c76.png align="center")

Now every time a test is executed which involves this flow of the program, a real payment has to be made to verify whether the application is working fine or not. But this is not an ideal and feasible approach to test the application.

And this is where the [Fakes](https://canro91.github.io/2021/05/24/WhatAreFakesInTesting/) must be brought in for testing. A fake is an alternate implementation of any service or module that's created and used when the actual implementation is impractical or not feasible to use. And here, the Payment Gateway will be replaced by its fake i.e. the Payment Gateway Fake.

![Fake Gateway connection](https://cdn.hashnode.com/res/hashnode/image/upload/v1690378853175/964551e9-9345-46f9-ab89-68cf79eb474d.png align="center")

This fake works exactly as the real payment gateway with the slight difference that it doesn't make the payment transfer. It's hard to create a fake because it must operate the same as the module it is replicating with some slight differences. Fakes enhance the performance of tests by not including and executing the complex modules due to which it was created in the first place.

### Points To Be Noted!!

The following points must be remembered under these objects:

1. Stubs -
    
    a. It isolates the actual code from dependencies while testing.
    
    b. It is easy to implement.
    
    c. Used to simulate real-world objects.
    
2. Mocks -
    
    a. It verifies the interaction between the modules and services used in an app.
    
    b. Might be hard to implement these sometimes.
    
    c. Used to verify the code behaviour.
    
3. Fakes -
    
    a. It's an alternate implementation of a dependency that's used when the actual one is not feasible to use.
    
    b. It is hard to implement.
    
    c. It improves the code performance.
    

### Conclusion.

Each of these objects i.e. stubs, mocks, and fakes play a unique role in creating an efficient and comprehensive testing strategy. Leveraging the strengths of these test doubles enables developers to build robust and maintainable code, promoting high-quality software development and enhancing overall software reliability. By selecting the appropriate test double for each testing scenario, developers can achieve better test coverage, faster testing cycles, and greater confidence in the correctness of their software.