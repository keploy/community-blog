---
title: "Mock vs Stub vs Fake: Understand the difference"
datePublished: Tue Feb 13 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clssrs4tj000p0al8gtbs48by
slug: mock-vs-stub-vs-fake-understand-the-difference
canonical: https://keploy.io/blog/community/mock-vs-stub-vs-fake-understand-the-difference
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707585234655/2ea8b5cc-0681-437e-b39f-be378de2bd24.png

---

## Introduction

Testing software is like putting it through a series of challenges to make sure it's tough enough for real-world use. Whether we're testing each piece individually (unit testing) or how they all work together (integration testing), we need to be prepared for different situations.

Sometimes, testing is tricky.There are times, when we need to isolate parts of our code from their dependencies. Also When we examine how different pieces of our application interact with each other.That's where tools like Mocks, Stubs, and Fakes come into the picture.

In this Article, We'll explore What is Fakes,Stubs And Mocks and their differences.

So without delaying further, Let's dive deep into it!

## **Fakes Vs Stubs Vs Mocks**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707286226829/b92297d7-cbd7-4457-9a5b-2b81735b9937.png align="center")

Fakes are a way for us to simulate objects and method functionality in our code without actually depending on a real object or method.

Stubs are a type of fake that quietly fakes behavior and can return a predefined expected value, while a mock is a type of fake that we can monitor to make sure that a certain method was called an expected amount of times or with an expected set of parameters.

In conclusion, while all three—Fakes, Stubs, and Mocks—serve as valuable tools in software testing, their differences lie in their roles. In the following sections we'll explore more about this.

## What Is Mock Testing?

Mock testing is a testing technique that is used in software development to simulate the behavior of components that a software module depends on.

Let's say we have a function that sends an email to a user when they register on a website. In our unit test for this function, we want to ensure that the email is sent correctly without actually sending an email every time we run the test.

Instead, we can use a mock email service that replicates sending emails. It helps us to check if our program is working correctly or not!

## What Is a Stub?

In simple words, A Stub is a simplified version of a software component used in testing that imitates the behavior of a service or object.

Suppose, We're developing a software application that relies on a database connection, but the database system isn't ready yet.

Now we'll create a stub function that simulates the behavior of the database connection without actually performing any database operations. This stub function will return dummy data or predefined responses to mimic the expected behavior of the real database connection.

Thus, we can work on the testing part without thinking much about the depenencies.

## What is Fake?

Just like stubs, A fake is a simplified version of a real component used in testing to mimic its behavior. Fakes typically have working implementations of all methods but may simplify complex behaviors or dependencies to make testing easier.

For example, a fake database might store data in memory instead of a physical database, or a fake HTTP client might return predefined responses instead of making actual network requests. Fakes help isolate code for testing and make tests faster and more predictable.

## **Why Do We Need Fake, Mock, and Stub?**

When the complexity of the project increases, the need of Fake, Mock and Stub increases.Testing applications with intricate dependencies can be challenging for testers.

Fakes, Mocks, and Stubs offer valuable tools for simplifying this process,It enables more effective testing practices and improves the quality and reliability of software applications.

By understanding the distinctions between these concepts and leveraging them effectively, We, developers can make test our softwares easily.

## Conclusion

If you found this blog post helpful, please consider sharing it with others who might benefit. You can also follow me for more content on Javascript, React, and other web development topics.

To sponsor my work, please visit: [Arindam's Sponsor Page](https://arindam1729.hashnode.dev/sponsor) and explore the various sponsorship options.

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

Thank you for Reading :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707304021607/2bf942ef-b3a2-4db2-8699-4573c661cac1.png align="center")