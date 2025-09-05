---
title: "Understanding Sanity Testing: A Practical Guide for Modern Development"
seoTitle: "Sanity Testing: A Beginner's Guide"
seoDescription: "Discover the essentials of sanity testing: a quick, focused test ensuring minor code changes haven't introduced new issues in software development"
datePublished: Fri Sep 05 2025 06:42:32 GMT+0000 (Coordinated Universal Time)
cuid: cmf6gwxem000c02jrhna54txv
slug: what-is-sanity-testing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754992629418/d0e6a962-151f-4fb5-8141-628693f5c111.jpeg
tags: software-testing, sdlc, smoke-testing, sanity-testing, smoke-vs-sanity-testing

---

Even the smallest code changes can carry hidden risks. A minor bug fix may end up breaking a completely different part of the project. That’s where sanity testing comes in. It provides teams a quick, focused approach to ensure that recent changes didn’t introduce new problems.

In this blog, we will outline what sanity testing is, when it is used, how it is accomplished, and why it is important when working in fast-moving development cycles.

## What is Sanity Testing?

It is a type of [software testing](https://keploy.io/blog/community/testing-methodologies-in-software-testing) that verifies certain changes made to an application after small updates, patches, or bug fixes. Its purpose is to quickly determine that the recent changes or fixes work as expected without the need for a full application test.

While comprehensive testing looks at the overall application, this process looks only at the parts of the application that changed. This allows developers and testers to save time in the development cycle, by catching obvious mistakes at the beginning.

It is usually a [manual](https://keploy.io/blog/community/why-manual-testing-matters-a-ultimate-guide-to-software-testing) , targeted approach that gives quick feedback to development and QA.

## Objectives of Sanity Testing

* Verify functionality after small changes
    
* Make sure rapid stability before further tests are performed
    
* Save testing effort by not performing full regression unnecessarily
    
* Act as a checkpoint to detect breakages early
    
* Give confidence to [QA](https://keploy.io/blog/community/qa-automation-revolutionizing-software-testing) and developers to continue with further testing.
    

## When Should Sanity Testing Be Conducted?

* A bug has been patched and requires verification prior to deployment.
    
* A minor upgrade or improvement has been provided to an existing feature.
    
* You receive a build concentrated on specific modules, and not the entire system.
    
* The deadline is very close, and complete regression testing is not possible.
    
* Developers require a quick confirmation prior to merging or releasing.
    

## Process of Sanity Testing

1. **Review and Scope Identification**  
    Start by reviewing what has been changed in the new build. The QA team checks release notes, bug reports, and code commits to identify which areas of the application require focus.  
    By doing so, testing is restricted to those areas only, without checking unrelated features unnecessarily.
    
2. **Selection of Test Cases**  
    Relevant test cases are chosen or created. They are for the functionality added, modified, or fixed.
    
3. **Test Execution**  
    Next, the testers run a short, concentrated batch of test cases against areas that have been identified.  
    Such a test is shallow and quick, to catch any major issues early on without doing complete regression testing.
    
4. **Result Evaluation**  
    Lastly, the tests are run and the test results are analyzed. When all tests have passed and changes work as they should, the build is approved to proceed to more advanced testing phases.
    
    Otherwise, if some critical issues are identified, the build is rejected and returned to be fixed, so that no unstable build should occupies time and resources in later testing.
    

![Flowchart illustrating the process of sanity testing. The steps include: Identification, Test Case Selection, Test Execution, and Result Evaluation, connected by arrows.](https://cdn.hashnode.com/res/hashnode/image/upload/v1753940889783/97871064-3e87-46d0-a919-fb2f4945253b.jpeg align="center")

## How to Perform Sanity Testing

Conducting does not need a large test plan. It is simple and requires a structured approach:

1. **Understand the Changes:** Start by fully understanding what has changed in the application. Go through change logs, bug reports, or enhancement requests to know which areas should be tested.
    
2. **Identify Critical Paths:** Determine the most critical user paths and core functionalities that might be affected by the changes. Pay attention to the features with the highest user engagement.
    
3. **Test the Modified Functionality:** Begin testing with the specific functionality that was fixed. Check if it is working as required and meets the specifications.
    
4. **Test Related Features:** Examine closely related features that may be affected by the modifications. This ensures that any unwanted side effects are noticed.
    
5. **Perform Basic User Scenarios:** Execute common user scenarios to check that main functions of the application are intact and accessible.
    
6. **Document Issues:** If defects are discovered during sanity testing, record them explicitly and report them to the development team for urgent resolution.
    

![Diagram illustrating sanity testing process with icons and labels: "Understand Changes," "Identify Critical Paths," "Test Modified Functionality," "Test Related Features," "Perform Basic User Scenarios," and "Document Issues," ](https://cdn.hashnode.com/res/hashnode/image/upload/v1754147273367/7e03d73c-6b3e-4949-94e1-feca293bc4a0.png align="center")

## Sanity Testing Example

Think about a banking app where developers address a bug that was causing the mobile check deposit to process deposit amounts incorrectly. Sanity testing would concern itself with the following:

* **Check Deposit Functionality:** Testers capture images of various check types, checks if the amount recognition is working correctly, and ensure that deposits are processed for accurate values, including checks for different formats, handwritten amounts, and printed amounts.
    
* **Account Balance Integration:** Since check deposits impact the account balance, testers ensure that the corrected deposit amounts suitably update the user account balances and that there is no discrepancy in the available funds calculations.
    
* **Transaction History Verification:** Testing would be extended to features directly related to deposits, such as transaction history display, pending deposits notification, and updates to the mobile banking dashboard.
    
* **Related Banking Features:** Testers would see if the deposit fixes affect other account features such as automatic savings transfers, overdraft protection calculations, or mobile payment capabilities.
    

This focused approach ensures the bug fix is applied correctly while verifying that other functionality stays stable.

### **Features and Attributes**

* **Limited Scope:**
    
    Covers a narrow range of test cases , targeting only to the relevant modules.
    
* **Simple to Design and Execute:**
    
    The tests are usually easy to create, addressing only primary functionalities that require confirmation after updates or patches.
    
* **Fast and Efficient:**
    
    These tests are quick and meant to give almost immediate feedback on whether further testing should proceed.
    
* **Part of the Regression Method:**
    
    While separate, sanity testing is actually a subset of [regression testing](https://keploy.io/blog/community/regression-testing-an-introductory-guide), offering a high-level check after updates.
    
* **Minimal or No Documentation:**
    
    Due to its informal and fast nature, sanity testing might not involve detailed documentation.
    
* **Handled by QA/Test Engineers:**
    
    It is usually conducted by the QA team to determine if the application is stable enough to proceed to more detailed testing phases.
    

## Advantages

* **Risk Mitigation:**
    
    Detecting key issues early prevents them from getting into later test phases or production environments, where they would be more expensive to correct.
    
* **Better Release Quality:**  
    By catching issues early, it allows for higher quality releases and improved customer satisfaction.
    
* **Time Efficiency:**
    
    Sanity test gets done quickly, thus providing rapid feedback on the recent changes and avoids any long testing cycles.
    
* **Integration with Development Workflow:**
    
    It naturally fits into modern development methodologies and supports continuous integration and deployment pipelines.
    
* **Early Issue Detection:**  
    Potential problems identified early are less expensive to fix compared to those detected during later testing phases or production environments.
    
* **Cost-Effectiveness:**  
    By targeting key areas, overall testing effort and cost is reduced hence validating the quality without compromising on quality assurance.
    

## Disadvantages

* **Limited Coverage:**
    
    Since the scope of the testing can be quite narrow, issues may go undetected until further testing phases.
    
* **Possibility of Missing Integration Issues:**
    
    Complex integration problems spanning across system components might be ignored while looking at specific areas.
    
* **Tester Expertise Required:**
    
    Being unscripted, it requires experienced testers who know about application architecture and areas of potential impact.
    
* **Risk of Over Reliance:**
    
    Teams may become excessively reliant on sanity testing, potentially leading to less investment in strategic and effective testing approaches.
    
* **Documentation Issues:**
    
    Due to minimal documentation, the information can become partially lost, thus creating knowledge gaps for future cycles or new team members.
    

## **Sanity Testing Tools**

* **Selenium WebDriver**  
    One of the most popular web application automation tools, [Selenium WebDriver](https://keploy.io/blog/community/introduction-to-selenium-software-testing#google_vignette) helps testers to build test cases targeting particular UI components. It makes sure the tests are adaptable to recent changes so that the verification of the features being changed can be done quickly.
    
* **TestNG and JUnit**  
    [TestNG and JUnit](https://keploy.io/blog/community/testng-vs-junit-performance-ease-of-use-and-flexibility-compared) are both testing frameworks for Java-based applications allowing structures test cases using features such as grouping, prioritization, and parallel execution of tests, allowing for much faster and structured test cycles.
    
* **Postman**  
    A good choice for [API testing](https://keploy.io/api-testing), Postman allows teams to quickly validate the backend with any recent changes. Test collections can be generated and reused for subsequent verification of  
    endpoints that have been affected so services are tested in a focused manner.
    
* **Cypress**  
    [Cypress](https://keploy.io/blog/community/exploring-cypress-and-keploy-streamlining-test-automation) presents a modern approach toward front-end testing. The tests execute swiftly with real-time reloading and intuitive debugging, , all of which make it an excellent choice for focused testing of web applications after minor code changes.
    
* **Jenkins**  
    Being one of the most common continuous integration tools, Jenkins is essential for embedding automated verification within automated pipelines. By executing tests for each build, Jenkins provides checks for new code commits early on before proceeding in the deployment process.
    

Of course, the list goes on, depending on your stack and testing requirements. Here's a broader overview of the ecosystem:

![Diagram showing testing tools categorized into three sections: "Test Execution Tools" with Selenium, Postman, Appium; "Test Frameworks & Automation Libraries" with JUnit, TestNG, Mocha; and "CI & Reporting Ecosystem" with Jenkins, TestRail.](https://cdn.hashnode.com/res/hashnode/image/upload/v1754156324840/68db4fd3-66dd-415d-8015-3064486bd56f.jpeg align="center")

### **Difference Between Smoke Testing and Sanity Testing**

Smoke testing and sanity testing are both quick checks that are conducted during software testing, but they have slightly different purposes and are conducted at different stages.  
[**Smoke testing**](https://keploy.io/blog/community/developers-guide-to-smoke-testing-ensuring-basic-functionality) is a high-level test that ensures that the fundamental and key features of a new build are functioning correctly. It is also known as a **build verification test** and performed before doing any extensive testing.  
**Sanity Testing**, on the other hand, is narrow testing conducted after making changes or fixing bugs on a stable build. It ensures that particular functionality is working as it should without repeating the whole application's verification.  
Both saves time by catching defects early, though they differ in scope, timing, and intention

### **Comparison Table:**

| **Aspect** | **Smoke Testing** | **Sanity Testing** |
| --- | --- | --- |
| **Definition** | A high-level test to verify critical system functionalities. | A narrow regression test to ensure a particular functionality. |
| **Purpose** | To verify the stability of the build for further tests. | To check if a bug fix or a change really took effect. |
| **Test Level** | System-level or integration-level. | Subset of regression testing at the module or function level. |
| **Scope** | Broad and shallow. | Narrow and deep. |
| **Performed By** | QA or development team. | Mainly QA team. |
| **Test Case Basis** | Based on major functionalities. | Based on detailed requirements or defect reports. |
| **Automation Feasibility** | Highly automatable because of its repetitiveness. | Partially automatable; manual intervention may be needed. |
| **Execution Time** | Fast; minimal test suite. | Faster than full regression but more detailed than smoke. |
| **When Performed** | After each build deployment. | After minor changes or defect fixes. |
| **Test Coverage** | Covers end-to-end basic workflows. | Covers specific functionalities or components. |
| **Build Rejection Criteria** | Build is rejected if smoke test fails. | Build is rejected if sanity test fails for the modified area. |
| **Example Scenario** | Login, homepage load, simple navigation. | Validating a fixed issue with password reset functionality. |

## **Role of Sanity Testing in the SDLC**

Sanity testing plays a vital part in maintaining software reliability during the Software Development Life Cycle (SDLC). Its timely application contributes to stability, speed, and process efficiency across different stages:

* **Development Phase Integration** During development, sanity testing provides instant feedback on code changes, allowing developers to fix problems while context is still fresh in their minds.
    
* **Deployment Preparation** It prior to production deployments ensures that key changes work as intended in environments closely simulating production conditions.
    
* **Continuous Improvement** It leads to results that return to development processes, indicating common patterns of issues and enhancing the development processes.
    
* **Quality Assurance Gateway** The testing acts as a quality gate between completion of development and extensive testing phases to ensure that the stable builds only move to detailed testing.
    
* **Maintenance and Support** For production applications, where patches and hotfixes are required, sanity testing ensures emergency releases do not compromise the software further from the end-user perspective.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1754992172360/c46d63fe-9bff-4f65-a508-16a2e46c57dc.png align="center")

## **Keploy for API Sanity Testing**

In fast-moving development cycles, [APIs](https://keploy.io/api-testing) are often the first place where bugs or regressions surface. A small change in an [API](https://keploy.io/blog/community/test-case-generation-for-faster-api-testing) response or dependency can break downstream services without warning. This makes **API sanity testing** critical, and Keploy provides a modern way to achieve it.

![keploy](https://cdn.hashnode.com/res/hashnode/image/upload/v1755754476123/d7ce4843-d32b-495c-955b-57df3f9ba756.png align="center")

In this blog, we’re talking about sanity testing. Once you’ve fixed the APIs, how do you check if they’re working fine or not? We manually write test cases and verify them using assertions, right?

That’s fine for small projects, but what if there are so many APIs? How do you test them all? And what about the edge cases for the APIs?

Why worry when Keploy is here for [API testing?](https://keploy.io/docs/running-keploy/generate-api-tests-using-ai/) Keploy provides you with a platform to create API test cases without writing any code or interacting with any SDKs. You heard it right the Keploy API Testing platform gives you API test cases that work for your application, including edge case scenarios too.

Curious about how it works? All you have to provide is:

1. cURL commands or a Postman collection
    
2. An OpenAPI schema
    
3. Your application URL (localhost also works)
    

Keploy will automatically create your API test cases and verify the test cases by running them against your application. In the end, you’ll have test cases that work for your application, and you can also run API testing in your [CI/CD pipeline.](https://keploy.io/docs/ci-cd/github/)

So, why wait? Go to [app.keploy.io](http://app.keploy.io) to create your test cases. Trust me, you will definitely like it.

## **Conclusion**

We covered a lot today; and by now, one should be able to grasp the concept of sanity testing quite well. Though often neglected, it is an important process, for it ensures quickly that the vital part of an application has not been broken by recent changes. Thus, it would give a necessary confirmation that the application is stable enough to proceed rather than plunging into full-fledged testing right away.

By catching key issues early , it saves time and reduces costly delays. Introducing it into your workflow will assure timely releases and greater confidence in your software. Adopting this practice strengthens your development process and ultimately delivers better products for your users.

## References:

[Qa Automation: Revolutionizing Software Test](https://keploy.io/blog/community/qa-automation-revolutionizing-software-testing)[ing](https://keploy.hashnode.dev/how-cicd-is-changing-the-future-of-software-development)

[Regression Testing To](https://keploy.hashnode.dev/how-cicd-is-changing-the-future-of-software-development)[ols: Ensuring Software Stability](https://keploy.blogspot.com/2025/03/regression-testing-tools-ensuring.html)

[Developer’S Guide To Smoke Testing : Ensuring Basic Functionality](https://keploy.io/blog/community/developers-guide-to-smoke-testing-ensuring-basic-functionality)

[Regression Testing Tools Rankings 2025](https://keploy.io/blog/community/regression-testing-tools-rankings-2025)

## FAQs

### 1\. I just fixed a bug. ShouId I do sanity testing, smoke testing , or both?

If the fix is limited to one feature, then sanity testing is sufficient. But for a major change that affects several areas of the product, conducting both smoke and sanity tests would be a smart move to verify the build.

### 2\. How many test cases should be there during sanity test?

You don’t need many. Just the minimum number that touches the updated part of your code. It is not about coverage, it is about speed and confidence.

### 3\. Can I skip sanity testing because I am short on time?

Actually, sanity testing will save you time. It is quick and its job is to stop you from wasting time executing full test suites on a build that has stability problems.

### 4\. Should sanity testing be manual or automated?

Depends on how often that area changes. For quick checks or one-time tests, manual is fine. But if you’re hitting it often, automating sanity tests offers a clear benefit as it’s faster, fewer mistakes, and saves you from repeating steps.

### 5\. What if my sanity tests pass but something breaks in production?

That can happen. It is not the end of your testing process; it basically shows you that your changes did not break anything obvious but for the coverage you will still need regression/full system testing.