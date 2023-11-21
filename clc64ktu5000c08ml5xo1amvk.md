---
title: "What is unit testing anyways?"
datePublished: Tue Dec 27 2022 11:08:41 GMT+0000 (Coordinated Universal Time)
cuid: clc64ktu5000c08ml5xo1amvk
slug: what-is-unit-testing-anyways
canonical: https://keploy.io/blog/community/what-is-unit-testing-anyways
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1671825687701/c2660d93-616e-4c0d-9f6c-047dc4ea2855.jpeg
tags: unit-testing, testing, test-driven-development, software-testing, methodology-and-types-of-software-testing

---

## Introduction:

Unit testing is a technique of testing that emphasizes testing a particular piece of code. Unit testing is mainly done to test the functionality of a block of code i.e to check whether the code does what you expect it to do.  
But does that mean we have to test every functionality individually? An application has numerous functions, wouldn't writing test cases for each of them increase the development time?  
Let's check this out in detail.

### What exactly is a "unit" in unit testing?

A unit can either be a class or an interface, functionality or even an entire package/module.

It is the smallest piece of code that can be tested in **isolation.**

The word “isolation” has been emphasized here since a test case is not considered to be a unit test if the test cases run parallel to other test cases. 

## The idea behind unit testing:

The primary goal behind unit testing is to isolate a chunk of your code and then verify if that piece of code works as it is expected to work or not.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671826157887/a41d6acf-78e4-4139-a650-28d817a6e921.png align="center")

Unit testing is the first level of testing and is done mostly by the developers who have developed the application since developers are the ones who have first-hand knowledge about the logical execution of a piece of code.

For example, let us consider that we have an eCommerce application with functionalities like product filtering, payment gateway, login logout system etc. Therefore, to test let’s say the product filtering function, you have to make sure that the function gives you the same set of output based on a given range of colors, brands and prices. This can be done by writing another function that calls the product filtering function with different input parameters and would then check whether the out given by the function and the output that we expect it to give are the same or not.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671826424475/28ab12bb-0c92-4e6c-b467-32dbfe6ba717.jpeg align="center")

The developers mostly use unit testing as a means of validating the functionality or the logical execution of the code. Since there can be several functionalities in your application, you might need to create several unit test cases to cover the execution of each function. Therefore the developers need to keep in mind that the unit test does not slow down the performance or increase the development time of the code. It is due to this reason that a test case is not considered to be a unit test if it talks to the database or if it is dependent on the file system since this would slow down the performance.

Unit testing focuses only on a particular piece of code that should be tested in isolation.  
It is due to this reason, that a unit test case has a very precise extent of assessment. Unit test cases target a single piece of code, and in case the test fails, we know exactly where the fault is coming from. This in turn helps in looking for bugs in the application and fixing them during the early stage of its development.

### Pros of unit testing:

* Through unit testing, developers can detect and fix the bugs during the early stages of development which increases the efficiency of your code. Refactoring of code becomes much easier if you have unit tests in place.
    
* Detection of errors in the early stages helps in eliminating the bugs which could lead to major issues in the future stages of development. This as a matter of fact, also saves a developer's time.
    
* Unit testing also enhances the readability and understandability of code by the developers.
    

### Cons of unit testing:

* Writing unit tests for each functionality can be cumbersome and time-consuming.
    
* Even though unit testing is done for chunks of code in isolation, not all errors are detected. Some errors might occur when the different modules interface with each other. These errors are mostly detected during the integration testing phase.
    
* Since unit testing tests the logical implementation of a code, it cannot test the application for its non-functional characteristics such as usability, scalability and performance
    

## Techniques of unit testing:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671826637775/dfb2afee-2dfa-4078-ade8-6f9b3181cb6c.jpeg align="center")

Unit testing techniques are of 3 types.

* **White box testing:** Also known as glass box testing, transparent testing or structural testing. This approach requires an in-depth knowledge of the internal structure of a function. Thus this technique allows the testers to validate the logical aspect of a function i.e to test the behavior of a function.
    
* **Black box testing:** In this approach, test cases are designed without any knowledge of the design or internal structure of the program. The test cases are designed entirely by analyzing the input/output behavior of the function. This approach is also known as function testing since the test cases are designed only based on the functional specification of the software. Test cases are designed entirely from the user’s point of view.
    
* **Gray box testing:** Also known as semi-transparent testing, this approach is a mixture of both black box testing and white box testing (hence the name gray box testing). Here, the testers know the internal structure and its working. However, this knowledge is quite limited.
    

## Myths of unit testing:

### **“Unit testing increases the burden on a project”**

While a part of it might be true, Unit testing your product would save your product from having major bugs and issues in the future development stages of your product. Since unit testing increases code reusability, it is in turn saving your development time and effort.

### **“It’s a simple piece of code, therefore it does not need testing”**

Again, a part of it might be true. However, you never know where the error may come from in the future. The code might seem simple to you, until and unless something goes wrong. Defining test cases for even the simplest piece of code would add stability and security to your product.

## Conclusion:

Unit testing is said to be the heart of the development lifecycle. Help you in detecting bugs in the early stages of development and save time, effort and development cost. Therefore, unit testing is a crucial part of any project. There are other levels of testing as well (Integration testing, end-to-end testing etc.), however, unit testing is the core of all testing levels.