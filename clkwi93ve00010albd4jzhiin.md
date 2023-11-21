---
title: "E2E Testing Guide: Beyond Unit Tests for Full Coverage"
datePublished: Mon Jul 31 2023 11:30:57 GMT+0000 (Coordinated Universal Time)
cuid: clkwi93ve00010albd4jzhiin
slug: e2e-testing-or-unit-testing-difference
canonical: https://keploy.io/blog/posts/e2e-testing-guide-beyond-unit-tests-for-full-coverage
tags: tdd, bdd, unit-testing, apis, testing

---

There is often a philosophical debate about whether to write a unit test or an end-to-end test. This has been a common question I have encountered many times - when limited in time and resources, what kind of testing should be done? Weighing up such factors can be difficult, especially given that usually only one type can be conducted.

Let's *start with why:*

## **Why do we Test?**

The primary objective of tests is to instil confidence that our code functions correctly. By detecting bugs before they impact users, tests act as a protective layer, ensuring the reliability of our software.

When a feature malfunctions, your tests act as detectives, swiftly identifying the root cause. Pinpointing the exact location of the issue allows you to analyze and rectify the problem efficiently. The more precise and granular our tests are in pinpointing issues, the faster we can resolve them.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685427159101/a7392d7e-743b-4aea-ac95-427f594a3bf5.png align="center")

# Unit tests

They are your first line of defence against bugs. A unit test typically has no dependencies, can be executed in milliseconds, and is perfectly reliable and if those units work as expected, It focuses on individual units of work instead of whole applications. **The unit of work can be single:**

* **function or method,**
    
* **class,**
    
* **module.**
    

### **Do I need them?**

Whether it be a function, a class, or anything else, unit tests are meant to test every feature/potential logical path. When writing unit tests, it is essential to cover every feature and potential logical path of the code, regardless of whether it is a function, a class, or any other component. Achieving 100% code coverage for the unit tests means that every possible flow within the function, class, or module has been thoroughly tested.

However, in reality, achieving 100% code coverage can be quite challenging and may not always be feasible. Realistically, a good target for code coverage is around 80%. This means that approximately 80% of the code is tested, taking into account different scenarios and logical paths.

%[https://twitter.com/erinfranmc/status/1148986961207730176?s=20] 

# **Mostly E2E Tests**

An end-to-end test is a test that exercises an aspect of the system within the context of the system as a whole. This test not only verifies that the feature works, but also verifies that it works under typical operational conditions.

The end-to-end test is essential, for example, If I want to know that the system I'm delivering meets the requirements and will continue to meet the requirements as the system evolves in the future, It allows me the opportunity to study the application under conditions where these bugs arise and is invaluable for building robust and reliable software systems.

## **How to write more E2E tests**

E2E tests are complex and difficult to implement. They are most common in production-grade code, so you can probably spare yourself the time on pet projects or schoolwork if manual testing is more efficient. In summary, use E2E tests when:

* There will be users aside from yourself
    
* You have enough infrastructure for a staging or dev environment
    

![](https://i.imgflip.com/7nkxk9.jpg align="center")

## How much end-to-end testing is necessary?

When it comes to the issue of how much end-to-end testing to perform, the go-to is often the Test Automation Pyramid. This model shows that a majority of automated tests should be unit tests. These are tiny but fast tests that are executed lower than the user interface layer. Then, there are API/integration tests in between, verifying a combination of elements such as data services, APIs and external services. Finally on top is E2E testing which is generally done through UI control. Thus, this pyramid displays how these kinds of tests should be performed for them to be most effective.

> For an accurate picture of the test automation pyramid, Mike Wacker of Google proposed a 70/20/10 split in 2015. This ratio stands for 70% unit tests, 20% integration tests, and 10% automated E2E tests.

# Conclusion

I don't think that anyone can argue that testing software is a waste of time. The biggest challenge is knowing what to test and how to test it in a way that gives true confidence rather than the false confidence of testing implementation details.

Whenever possible, I try to write more E2E tests and view unit tests as complimentary to E2E tests. **As you move up the pyramid, the confidence quotient of each form of testing increases.** So while E2E tests may be slower and more expensive than unit tests, they bring you much more confidence that your application is working as intended.

Overall, it ultimately comes down to whether its purpose/common use cases suit you, and whether their properties (i.e. execution time, required technical infrastructure, etc) are viable for your system.