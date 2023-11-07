---
title: "Frustrations of API Testing"
seoTitle: "Frustrations of API Testing"
seoDescription: "Sometimes API Testing becomes so frustrating!"
datePublished: Thu Aug 04 2022 15:55:47 GMT+0000 (Coordinated Universal Time)
cuid: cl6f7zjam01t9x5nv231f5j74
slug: frustrations-of-api-testing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1657750872493/kDGZvl_Bw.png
tags: apis, frustration, api-testing, keploy, api-development

---

Hello!

Sometimes API Testing becomes so frustrating.

Every person who's doing API Testing has to go through it eventually. And there is no other way other than to go through the phase and find a solution.

As a tester I have experienced some of these frustrating moments in API Testing, and to know more on these I've compiled some of the biggest frustrations of API Testing from across the developer portals and communities.

In this article. I will brief about the biggest API Testing issues that testers face by sharing the stories and comments of developers.

---

![1_aRIDTkMCa1Fx9dDKMCpHlw.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1657750892827/UkHVNw7lv.jpeg align="center")

Going through most developer portals, I've divided these into 7 major frustrations:

1. Debugging the API request
2. Poor API Documentation
3. Incorrect response and status code
4. Performance issues
5. Security issues
6. API mocking is costly process
7. Finding flow and building flow is challenging

Let's see about each frustration and the comments of Developers on it.

---

### Debugging the API request
> Since any developer starts learningAPI testing, the biggest frustration with it maybe it is difficult to debug the failing API Tests

> The biggest frustration with API testing for one is that it takes so much time to write and debug tests. One might often ask oneself "If it takes so much time to write the test, is it worth it?".

These are some statements from a developer, stating the frustration of debugging a failing API test.

Debugging and troubleshooting APIs is something that any developer that works with APIs has to go through at some point. In an ideal world, APIs would always return a 200 OK with just the right data that we need, or in case of a failure, it would return us the perfect status code and error message allowing us to easily understand what went wrong.

But in reality, APIs don't always work like that. API developers might have constraints that do not allow them to implement the most informative status code or error message, API errors can be triggered by real-world conditions that are hard to account or test for, and sometimes ourselves, as API users, can make requests with typos or mistakes that APIs just don't know how to handle.

To handle this, API Developers have to go through a process of debugging the API to look after the failed Test Cases which is in general very hard to do.

---

### Poor API Documentation
> Developers really enjoy testing APIs, but let's face reality here: every job has its frustrations. One of the things that developers have found frustrating in API testing is when an API is inadequately documented.


> There is something fun about digging around and figuring out what paths there are in an API and how it works, but some APIs make this too hard.
> It is fun to find out what the API is and what it can do but you can only get so far on your own. Having Documentation that is only partly ready makes your life really not fun

> The dev community is a passionate one. It's specifically passionate about the things they don't like. If they don't like an API, it's most of the time because of junky docs, even if the product is actually great.
These are some statements from developers, stating the issues one faces with poor API documentation or no API documentation.

APIs make the world go round. Developers use APIs almost every day - by some estimates, they spend a whopping 10+ hours a week working with APIs. This covers not only using them, but also researching, googling for support, studying reviews, and of course, rummaging around in the documentation.

The top API pet peeve of any developer is likely related to poor documentation. It may be inaccurate, incomplete, or just plain absent. 

Creating an API documentation is a tedious task and some reasons why developers are 
1. lazy in doing it are:
2. lack of tools
3. error handling
4. creating a doc structure
5. writing the doc details
6. maintenance

---

### Incorrect response and status code
> Biggest frustration in the past has been incorrect status code e.g 200 rather than 404 and inconsistent response formats e.g different json format for errors and success.
>
> Using an API that returned an HTML error page instead of the JSON one expected, causing ones code to blow up. Or receiving a 200 OK status code with a cryptic error message in the response can really be frustrating

These are some statements from developers, stating the issues one faces with poor API documentation or no API documentation.

HTTP response status codes indicate whether a specific HTTP request has been successfully completed.

One needs to be careful and not assume that an API 200 status code means the request made a successful call and returned the information they want. Some APIs, like Facebook's Graph API, always return a 200 status code, with the error being included in the response data. So, when testing and monitoring APIs, always be careful and don't automatically assume that a 200 means everything is ok™.

---

### Performance issues
> After making the end to end framework and making sure it works fine, generally one tends to skip performance testing to check the servers.
> Due to this once the api is live, it gives slow response hence frustrating the developers

> Test cases taking a long response to run is also what one can consider frustrating sometimes.

Performance testing determines whether your APIs perform optimally under changing demands, or under various loads.
Lack of performance testing can end up making your api response slow and hence slowing down the application.
APIs should be functionally correct, as well as available, fast, secure and reliable. Hence next we will look at some frustrations on API Security issues.

---

### Security issues
> For one, it's APIs with hidden dependencies. A few developers recently ran into issues with one that required authorization and a small number of other properties before any of the published calls could succeed - but did not provide an endpoint to perform said authorization and property-setting.

Since APIs are the building blocks of a much larger application, not a lot of people think about authorization problems. Therefore, in an application with multiple levels of access, it's become common for a user of a lesser privilege accesses data or information of users belong to a higher level of privilege.

APIs are the pipes that connect various applications and (micro)services. As data flows through them, security is of utmost importance to prevent data leakage. Also, since APIs are like doors into ones application, they're the obvious entry point for attackers who want to break your system.

A breach in API security may result into exposition of sensitive data to malicious actors.API security is nothing but securing the API endpoints from attackers and building your APIs in a secure fashion. A vulnerable API could lead to:

- Unauthorized Access
- Data leakage
- Sanctioning Fuzzy input
- Injection Vulnerabilities
- Parameter Tampering, etc.

---

### API mocking is costly process
> Managing mocks is one of challenges when perform API testing. One is applying Scrum framework and Agile methodology to develop our software. The consequence of emerging products from Sprint to Sprint is Data modal changed. Therefore, managing and maintaining mock data is ones biggest frustration.

Mocks is nothing but an imitation. It simulates behaviour of real API but in a more a controlled manner. A simple mock server consist of a server, which on matching certain request returns pre-defined response and other parameters associated such as response code, headers etc. Using mocks leads to violations of the DRY principle.

API load testing does not simulate real users interacting with elements of your webpage and it doesn't measure front-end performance or how quickly pages render in different browsers.

---

### Finding flow and building flow is challenging
> What makes API testing so challenging is the large number of parameter combinations that have to be covered. The purpose of API parameters is to pass data values through API endpoints via data requests. Choosing the wrong API parameter combinations can trigger faulty program states that might potentially expose APIs to external attacks or cause crashes.

APIs allow for communication between different applications which are built on service-oriented architectures, all these APIs must be tested to ensure their functionality and usability, despite lacking a graphical interface and requiring a more programmatical approach, which can make the API testing flows more challenging and intimidating.

Due to large number of combinations, not all permutations can be tested which results to exposed APIs and issues generating while using the API.

---

That was all the frustration over API Testing summed up in one article!

Don't forget to give me a follow as I'll publish more such blogs related to API😁


 -by [Nishant Mishra](https://twitter.com/curlyParadox)