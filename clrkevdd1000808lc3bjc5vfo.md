---
title: "Master BDD Testing with Cucumber.js: A Comprehensive Guide"
seoTitle: "Learn BDD Testing with Cucumber.js | Comprehensive Guide 2024"
seoDescription: "Discover the power of BDD testing with Cucumber.js. Learn how to bridge the gap between business and development with practical examples."
datePublished: Fri Jan 19 2024 09:01:10 GMT+0000 (Coordinated Universal Time)
cuid: clrkevdd1000808lc3bjc5vfo
slug: bdd-testing-with-cucumber-js
canonical: https://keploy.io/blog/community/bdd-testing-with-cucumber
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705654804499/6a0dca55-e895-4db1-af31-c97edb420cee.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1705654828140/1ddf47a6-0ce9-4617-8615-c02b54dc64a8.png
tags: bdd, testing, cucumber, testing-framework, testing-tools

---

While BDD and Cucumber.js have been around for some time, many developers are still unfamiliar with how these tools can streamline development processes and improve software quality.

This comprehensive guide will help you understand the core concepts of BDD, how to implement it using Cucumber.js, and how this approach can benefit your projects by making automated tests more readable and maintainable.

## Understanding BDD: Business Driven Development Explained

BDD is short for [Business Driven Development](https://keploy.io/docs/concepts/reference/glossary/behaviour-driven-development/), it's way to close the gap between business people and technical people. Basically, BDD has evolved from TDD, there's a high chance that you might even be doing BDD without knowing it, as sometimes the lines between them aren't clear.

So what's the difference between them, both are **automated tests***right*? Well, the difference is that we use the language of our end-users, i.e. the business or domain language such as **Given-When-Then**, to capture like a story in an executable format. For example,

```plaintext
GIVEN User is on Wordpress Registration Page.
WHEN he enters all the required information.
THEN his account is created.
```

The “given” part is where we declare preconditions. In our example above we had a user. Next, the “when” part contains the action you want to test. And finally, you verify the outcome in the “then” part.

Now, If you have more than one or you require more information than this, you can add them with **AND**. For example,

```plaintext
GIVEN User is on Wordpress Registration Page.
WHEN he enters all the required information.
AND he hits ‘create account’
THEN his account is created.
AND his confirmation email is sent.
```

## What is Cucumber Testing?

Cucumber-js is **a** [**test framework**](https://keploy.io/docs/concepts/reference/glossary/cucumber-testing/) **that supports BDD**. The tests are written in the Gherkin language, which are human-readable and are stored in feature files that have the feature extension. This allows your tests to be a point of communication and collaboration with bussines people and can even serve as documentation that is automatically up-to-date.

A test in the Gherkin language is called a scenario. And scenarios are organized into features.

![Cucumber.js BDD Testing Guide.](https://cdn.hashnode.com/res/hashnode/image/upload/v1722828593040/44851fbc-2cf7-47f4-bbe1-0589a12152fd.png align="center")

In the above example, there are many **keywords**, and every keyword has its own meaning and purpose.

Let’s explain each keyword with respect to the above example.

* **Feature Keyword**: The feature file starts with the keyword Feature\*\*.\*\* Under feature, you can mention the feature name which is to be tested, as seen above.
    
* **ScenarioKeyword**: Each scenario must starts with the keyword Scenario\*\*,\*\* followed by the scenario name. And under each feature file, there can be more than one scenarios.
    
* **Given Keyword**: Given keyword is used when we have to give some pre-condition in our test case.
    
* **When Keyword**: When the keyword is used to perform some action.
    
* **And Keyword**: It is used to **connect** two statements with the logical **AND** condition between any two statements. It can be used in combination with **GIVEN**, **WHEN** and **THEN** statements.
    
* **Then Keyword**: Then Keyword is used for the final outcome or for validation.
    

Other than these, there are two more keywords,

* **ButKeyword**: This keyword is used to represent negative assertions in addition to the previous statement.
    
* **Background Keyword**: Steps kept under Background would be run before every test case.
    

Although, Initially Cucumber was created for '**Ruby Language**' as a support for **RSpec** BDD framework for testing, Cucumber-js has evolved to supports a variety of different programming languages such Java, JavaScript, PHP, Net, Python, Perl and etc...

### Benefits of Cucumber Testing

Cucumber testing offers numerous benefits that enhance the software development process:

* **Improved Collaboration:** Uses plain language to describe test scenarios, fostering better communication between technical and non-technical team members.
    
* **Living Documentation:** Serves as up-to-date documentation, simplifying onboarding and maintenance.
    
* **Aligns with Business Goals:** Bridges the gap between automated tests and business requirements, ensuring accurate development cycles.
    
* **Multi-Language Support:** Compatible with multiple programming languages, making it adaptable to various tech stacks.
    
* **Readable Tests:** Uses Gherkin syntax to make tests human-readable, which enhances understanding and collaboration.
    

### How Cucumber Testing Works ?

Cucumber testing follows a structured approach to ensure that tests are both readable and maintainable. Here’s a step-by-step breakdown of how it works:

* **Write Feature Files:** Feature files contain high-level descriptions of a software feature and include multiple scenarios that illustrate different aspects of the feature. These files use the Gherkin language to make the tests human-readable.
    
* **Define Steps:** Steps correspond to the actions described in the feature files. Each step in the Gherkin scenario (Given, When, Then) is implemented in code, typically using a language like JavaScript, Java, or Python.
    
* **Create Step Definitions:** Step definitions link the steps in the feature files to the actual code that performs the actions. These definitions translate the plain language steps into executable code.
    
* **Run Tests:** Use the Cucumber command-line tool to execute the tests. Cucumber reads the feature files and matches each step to its corresponding step definition, running the associated code.
    
* **Generate Reports:** After running the tests, Cucumber can generate detailed reports in various formats (HTML, JSON, etc.), providing insights into which scenarios passed or failed and highlighting any issues that need to be addressed.
    

## Writing Simple BDD Tests with Cucumber.js

We will create a few tests to see how cucumber works, for this we will first define our scenario and feature before starting to write our code. Let's get started!!

First, let's create our work directory and create our `package*.json` file: -

```bash
mkdir bdd & cd bdd
npm init -y
```

Now, it's time to install our dependencies

```plaintext
npm i chai @cucumber/cucumber @cucumber/cucumber-expressions cucumber-html-reporter
```

### Folder Structure for Cucumber.js

Let's move towards writing the tests and setting up folder structures for cucumber-js, this is how the folder structure should look like: -

```bash
├── Features
│   ├── Support
│   │   ├── steps.js
│   │   ├── world.js
│   ├── simple_math.feature
├── utils
│   ├── report.js
├── cucumber.js
├── node_modules
├── package.json
├── package-lock.json
└── .gitignore
```

Create a folder structure similar to above before moving forward. Once we have our folder structure ready, let's start with `steps.js` file: -

```javascript
const { Given, When, Then } = require("@cucumber/cucumber");
const { expect } = require("chai");

Given("a variable set to {int}", function(number) {
  this.setTo(number);
});

When("I increment the variable by {int}", function(number) {
  this.incrementBy(number);
});

Then("the variable should contain {int}", function(number) {
  expect(this.variable).to.eql(number);
});
```

In Cucumber-js, “**world**” is an isolated scope for each scenario. That means all the steps, hooks, etc., for a scenario, is a part of a world and this data can be accessed within the world, anytime. Let's define our `world.js` file

```javascript
const { setWorldConstructor } = require("@cucumber/cucumber");

class CustomWorld {
  constructor() {
    this.variable = 0;
  }
  setTo(number) {
    this.variable = number;
  }
  incrementBy(number) {
    this.variable += number;
  }
}
setWorldConstructor(CustomWorld);
```

Finally, we have both steps and world ready, now it's time to create our `.feature` file.

```plaintext
Feature: Simple maths
  In order to do maths
  As a developer
  I want to increment variables

  Scenario: easy maths
    Given a variable set to 1
    When I increment the variable by 1
    Then the variable should contain 2

  Scenario Outline: much more complex stuff
    Given a variable set to <var>
    When I increment the variable by <increment>
    Then the variable should contain <result>
    Examples:
      | var | increment | result |
      | 100 |         5 |    105 |
      |  99 |      1234 |   1333 |
      |  12 |         5 |     17 |
```

Above, we have defined our feature and in correspondence, created have two different scenarios. Let's run to see if the test pass/fail :

```plaintext
npx cucumber-js
```

The output we get would be similar to below:

![Cucumber.js BDD Testing Guide.](https://cdn.hashnode.com/res/hashnode/image/upload/v1705649177349/61bc2e8f-0e19-45eb-a0b5-9837ac6ec313.png align="center")

### Generating Test Report

Now to make it more understandable, let's add functionality to create the test-report for each run.

In `cucumber.js` file, we will add: -

```javascript
var _default = [
    '--format usage:reports/usage.txt',
    '--format json:reports/cucumber_report.json',
    '--format html:reports/cucumber_report.html'
  ].join(' ')
  
  module.exports = {
    default: _default,
  }
```

This will create a json and html report to visualizing and append in the same files, each time we run our tests. Now let's define what all be there in report file.

In `report.js` , we will add few lines to provide some more idea about test enviroments in form of metadata.

```javascript
var reporter = require('cucumber-html-reporter');

var options = {
        theme: 'bootstrap',
        jsonFile: 'reports/cucumber_report.json',
        output: 'reports/cucumber_report.html',
        reportSuiteAsScenarios: true,
        scenarioTimestamp: true,
        launchReport: true,
        metadata: {
            "App Version":"0.3.2",
            "Test Environment": "STAGING",
            "Browser": "Edge  54.0.2840.98",
            "Platform": "MacOS",
            "Parallel": "Scenarios",
            "Executed": "Remote"
        }
    };

reporter.generate(options);
```

Now run your tests once again, and you can see that under `reports/` folder there are two files with `cucumber_report.*` name. Open the **HTML** to see your test report html file would look something like below: -

![Cucumber.js BDD Testing Guide.](https://cdn.hashnode.com/res/hashnode/image/upload/v1705653728395/103d6810-2c70-477d-9d53-b0a7ebdc56bd.png align="center")

## Benefits of BDD

| **BDD Benefits** | **Description** |
| --- | --- |
| Collaboration | Aligns developers and non-technical stakeholders using natural language tests. |
| Documentation | Test cases double as up-to-date, living documentation. |
| Multi-Language Support | Works with Java, JavaScript, Python, and more. |

## Conclusion

Cucumber-js focuses mainly on making the collaboration between tech people and business people easy. But it doesn’t eliminate the coding part and still has a lot of code involved. When dealing with **behavior-driven** automation, there are a lot of rules that you have to use as guide, the main thing we have to keep in mind is how to properly use keywords GIVEN, WHEN, and THEN.

The testing industry has gone to the next step now with codeless testing integrated with AI and smart logic. Frameworks like [Keploy](https://keploy.io/) allow you to create tests without using a single line of code using record and playback testing. But if you need to still use code for some reason, it also allows you to integrate your code into the test project.

The bottom line is that you have 3 options to choose from for testing:

1. Traditional testing.
    
2. Code testing that’s easy for business people to understand.
    
3. Codeless, AI-powered testing.
    

What suits you best depends on your use case. But it’s important to know about all these options to decide what suits you best. So good luck exploring!

## FAQ's

### What is Gherkin ?

Gherkin is a simple, human-readable language used to describe the behavior of software in a structured format. Gherkin files typically contain scenarios written in plain language, outlining how the software should behave under various conditions, making it easier for both technical and non-technical stakeholders to understand and collaborate on software requirements.

### What is BDD testing and how does it differ from other testing methodologies?

BDD testing is an approach where software behavior is described in natural language, focusing on the expected outcomes rather than technical details. It differs from other testing methodologies like TDD by emphasizing collaboration among developers, testers, and non-technical stakeholders through shared understanding of user requirements expressed as scenarios.

### What are some best practices for writing effective BDD scenarios ?

By following these best practices, teams can create BDD scenarios that effectively communicate user requirements, facilitate collaboration, and improve the overall quality of software products: -

1. **Use Clear and Descriptive Language**
    
2. **Keep Scenarios Single and Independent**
    
3. **Use Background and Scenario Outline**
    
4. **Write Readable Step Definitions**
    
5. **Periodically Review and Refactor Scenarios**
    

### What some tool I can use to create BDD test ?

There are multiple tools that can help you out in creating the BDD tests similar to Cucumber.js such as: -

1. [**SpecFlow**](https://specflow.org/): SpecFlow is a BDD framework for .NET applications, primarily used with C#.
    
2. [Behave (Python)](https://behave.readthedocs.io/en/latest/) : It is a Python-based BDD framework that uses Gherkin syntax for writing feature files and step definitions.
    
3. [**Karate**](https://www.karatelabs.io/): Karate is an open-source BDD framework for testing web services and APIs. It uses a syntax similar to Gherkin and allows writing feature files to describe API tests.