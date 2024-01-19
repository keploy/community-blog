---
title: "BDD Testing with Cucumber"
seoTitle: "BDD Testing with Cucumber"
seoDescription: "Cucumber.js and BDD are not new, still many developers are fairly unfamiliar with them, the two together can be very powerful tools for both business & devs"
datePublished: Fri Jan 19 2024 09:01:10 GMT+0000 (Coordinated Universal Time)
cuid: clrkevdd1000808lc3bjc5vfo
slug: bdd-testing-with-cucumber
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705654804499/6a0dca55-e895-4db1-af31-c97edb420cee.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1705654828140/1ddf47a6-0ce9-4617-8615-c02b54dc64a8.png

---

Cucumber.js and BDD are not new, still many developers are fairly unfamiliar with them, the two together can be very powerful tools for both non-tech people and developers.

## What Is BDD?

BDD is short for Business Driven Development, it's way to close the gap between business people and technical people. Basically, BDD has evolved from TDD, there's a high chance that you might even be doing BDD without knowing it, as sometimes the lines between them aren't clear.

So what's the difference between them, both are **automated tests** *right*? Well, the difference is that we use the language of our end-users, i.e. the business or domain language such as **Given-When-Then**, to capture like a story in an executable format. For example,

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

## Introducing Cucumber

Cucumber is **a test framework that supports BDD**. The tests are written in the Gherkin language, which are human-readable and are stored in feature files that have the feature extension. This allows your tests to be a point of communication and collaboration with bussines people and can even serve as documentation that is automatically up-to-date.

A test in the Gherkin language is called a scenario. And scenarios are organized into features.

```plaintext
Feature: Automatic discounts for premium customers
    Premium customers should automatically get a
    discount of 25% on purchases over $500.

    Scenario: Purchase over $700
        Given a premium customer
        And an order containing
            | item   | amount | price |
            | pencil | 100    | 2     |
            | paper  | 10     | 35    |
        When the customer checks out
        Then the total price should be 412.5
```

In the above example, there are many **keywords**, and every keyword has its own meaning and purpose.

Let’s explain each keyword with respect to the above example.

* **Feature Keyword**: The feature file starts with the keyword Feature**.** Under feature, you can mention the feature name which is to be tested, as seen above.
    
* **Scenario** **Keyword**: Each scenario must starts with the keyword Scenario**,** followed by the scenario name. And under each feature file, there can be more than one scenarios.
    
* **Given Keyword**: Given keyword is used when we have to give some pre-condition in our test case.
    
* **When Keyword**: When the keyword is used to perform some action.
    
* **And Keyword**: It is used to **connect** two statements with the logical **AND** condition between any two statements. It can be used in combination with **GIVEN**, **WHEN** and **THEN** statements.
    
* **Then Keyword**: Then Keyword is used for the final outcome or for validation.
    

Other than these, there are two more keywords,

* **But** **Keyword**: This keyword is used to represent negative assertions in addition to the previous statement.
    

* **Background Keyword**: Steps kept under Background would be run before every test case.
    

Although, Initially Cucumber was created for '**Ruby Language**' as a support for **RSpec** BDD framework for testing, it has evolved to supports a variety of different programming languages such Java, JavaScript, PHP, Net, Python, Perl and etc...

### Writing simple BDD test with Cucumber

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

Let's move towards writing the tests and setting up folder structures for cucumber, this is how the folder structure should look like: -

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

In Cucumber.js, “**world**” is an isolated scope for each scenario. That means all the steps, hooks, etc., for a scenario, is a part of a world and this data can be accessed within the world, anytime. Let's define our `world.js` file

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

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705649177349/61bc2e8f-0e19-45eb-a0b5-9837ac6ec313.png align="center")

### Test Report File

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

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705653728395/103d6810-2c70-477d-9d53-b0a7ebdc56bd.png align="center")

## Conclusion

Cucumber.js focuses mainly on making the collaboration between tech people and business people easy. But it doesn’t eliminate the coding part and still has a lot of code involved. When dealing with **behavior-driven** automation, there are a lot of rules that you have to use as guide, the main thing we have to keep in mind is how to properly use keywords GIVEN, WHEN, and THEN.

The testing industry has gone to the next step now with codeless testing integrated with AI and smart logic. Frameworks like [Keploy](https://keploy.io/) allow you to create tests without using a single line of code using record and playback testing. But if you need to still use code for some reason, it also allows you to integrate your code into the test project.

The bottom line is that you have 3 options to choose from for testing:

1. Traditional all-code testing.
    
2. Code testing that’s also easy for business people to understand.
    
3. Codeless AI-powered testing.
    

What suits you best depends on your use case. But it’s important to know about all these options to decide what suits you best. So good luck exploring!