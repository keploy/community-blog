---
title: "What Problem Keploy Solves?"
seoTitle: "How Keploy automates E2E Testing"
seoDescription: "Open-source Tool for converting user traffic to Test Cases and Data Stubs."
datePublished: Mon Oct 16 2023 06:30:16 GMT+0000 (Coordinated Universal Time)
cuid: clnsindeg000b09l74bs2hvdq
slug: how-keploy-automates-e2e-testing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1696758063415/33166570-daf7-4c3d-bc97-4ba1555a01ee.png
tags: testing, test-automation, testing-library, testing-tool, keploy

---

Hey Everyone in this Blog, we will discuss What problem keploy solves, before that, let's discuss : -

### *What is Keploy 🤔?*

![keploy logo](https://camo.githubusercontent.com/703f03885b2eb91fb5ad2105584d96f189d42e6ed80b59a1ad6802ebde22b0ab/68747470733a2f2f646f63732e6b65706c6f792e696f2f696d672f6b65706c6f792d6c6f676f2d6461726b2e7376673f733d32303026763d34 align="left")

Keploy is an open-source no-code E2E Testing Platform that generates test cases and data mocks from real user traffic. Application testing is one of the biggest barriers to achieving truly Continuous Deployments because it's use-case specific. Developers often avoid writing test cases because it's time-consuming.

In a single sentence

> Open-source Tool for converting user traffic to Test Cases and Data Stubs

### Problem:

We have heard about a lot of tools that can help us automate our CI/CD Pipelines and make them secure but there is one major blocker still to achieving true continuous delivery which is **Testing** because it is still very use-case specific and not technology-specific means you can not write a test like a script which will work for every one of your applications and you need to spend efforts and time to write high-quality automation test Suites and sometimes even more than you spend on developing the applications and even after that you can not ensure that there is one metric that will ensure bug-free code and node effects leaking to production. That's the problem in achieving an autonomous testing cycle in the CD process.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696667294173/df052196-6886-433c-afb8-6fa0b62ba47d.png align="center")

### What do we all need to Automate this Testing Process?

There are three things, we all need to automate this testing process:

1. Auto-Generated Test Scripts
    
2. Easily Maintained Test Cases
    
3. Test Data = Real (Production) Data
    

That's all we need to automate the testing process.

### Different Solutions:-

Let's Discuss different solutions and what are the limitations of those:

* If you want to test in productions you just need to do that **shadow testing**.
    

### What is shadow testing🤔?

your application is running, serving the user traffic and you put the new version of let's say **V2** and try to replicate the same traffic via some services and you try asserting some responses of the previous expected and as well as the new deployment that you are going to make. if those are matching means all the things are working fine.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1696689707415/f18dbd6c-da2d-4e61-8ffa-97eca428376e.png align="center")

**But the problem** is that it works fine with stateless applications. For stateful applications, there are a lot of other dependencies that your application is talking to especially Databases and you can not the same state that you going to test.

### **How can we deal with that 🤔?**

Here Keploy comes into the picture, It generates E2E tests for APIs (KTests) along with mocks or stubs(Kmocks) by recording real-world API calls KTests can be imported as mocks for consumers and vice-versa

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1692827850952/224af688-ccd9-4687-9c2f-00daffbbfbc1.gif?auto=format,compress&gif-q=60&format=webm align="left")

![Features bg](https://www.keploy.io/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Frecord-replaye538dde2a6e078f1b5a5.4eb8ec3f.gif&w=1080&q=75 align="left")

also Integrates with the unit test library and tests the results on the **CLI**

![Features bg](https://www.keploy.io/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Freplay-tc31305e1d2286fe485b27.c48e86b0.gif&w=1080&q=75 align="left")

### Conclusion 👋:

In summary, Keploy is an E2E Testing Platform that automates the creation of test cases and data mocks from real user traffic, addressing the challenges of time-consuming and use-case-specific testing in continuous deployment.

It provides auto-generated test scripts, easily maintainable test cases, and the ability to use real production data for testing. Keploy can handle stateful applications, integrating with unit test libraries and CLI testing, making it a valuable tool for efficient and comprehensive testing in the CI/CD pipeline.

### Keploy Hacktoberfest :

![hacktoberfest-2023](https://raw.githubusercontent.com/Sonichigo/crdb_keploy/main/Simple%20Technology%20Blog%20Banner%20(1).png?raw=true align="left")

You can contribute to Keploy Hacktoberfest [Active issues](https://github.com/keploy/keploy/issues?q=is%3Aissue+is%3Aopen+label%3Ahacktoberfest2023). Make sure to give [Keploy](https://github.com/keploy) a Star and follow it on Social Media: - [Twitter(X)](https://twitter.com/Keployio) [Slack](https://keploy.slack.com/ssb/redirect) [Hashnode](https://keploy.hashnode.dev/) [YouTube](https://www.youtube.com/channel/UC6OTg7F4o0WkmNtSoob34lg)

And if like the content I write, feel free to follow me on [My Twitter](https://twitter.com/PrashantSony6)