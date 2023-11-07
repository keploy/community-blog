---
title: "Introduction to testing with Mocha & Keploy"
seoTitle: "Testing with Mocha and Keploy"
seoDescription: "Testing with Mocha and Keploy"
datePublished: Tue Aug 23 2022 15:52:00 GMT+0000 (Coordinated Universal Time)
cuid: cl76d7us102yx43nv3sux8myj
slug: introduction-to-testing-with-mocha-and-keploy
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1660837330828/9wbtUwv7fl.png
tags: unit-testing, javascript, apis, testing, keploy

---

![anything.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1660724997724/Tr5Q1zgDY.jpg align="center")

Hello developers! I hope you are all enjoying the process of writing test cases. Let out a sigh of relief.

No matter what type of application is being developed, it should always be tested before being released. Testing frameworks such as Jest, Jasmine, QUnit, Karma and Cypress are just a few of the many options available. **Mocha** is among the most commonly used testing frameworks for JavaScript.

## What is Mocha?

Mocha is a widely used JavaScript testing framework that provides a flexible and powerful environment for running tests. It is designed to be simple yet comprehensive, allowing developers to write test cases and assertions with ease.

Mocha supports multiple testing styles, including synchronous and asynchronous testing. This set of features is quite rich. It provides support for browser and Node.js environments, test coverage reporting, and customizing report formats.

With Mocha, developers can organize their tests into suites and use various hooks to manage test setup and teardown. Its popularity stems from its simplicity, and extensive community support, making it a go-to choice for testing JavaScript applications.

One of the key features of Mocha is its support for various testing styles and methodologies. It offers both synchronous and asynchronous testing capabilities, making it easy to handle different types of code structures. Mocha provides a rich set of built-in functions and APIs for organizing and structuring tests, including features like test suites, test cases, hooks, and assertions. This allows developers to write comprehensive test suites that cover different aspects of their application's functionality.

You can use it with many assertion libraries, usually with Chai. However, for this simple demonstration, I am using the assert library.

# Getting Started

You can check the official documentation for Mocha. It is pretty simple to install just:

```powershell
$ npm install mocha
$ mkdir test
$ $EDITOR test/test.js
```

Please create a folder called "tests" in the main directory of our project. We will use this folder to store all the code for our tests. It will make sure they are well-organized and distinct from other files.

### Let's create a file say sum.js and inside it a sum function in our root directory of the project:

```powershell
function sum(num1, num2){
    return num1 + num2;
}
exports.sum = sum
```

# Now let's create a sample test

First, open your /tests folder and create a new file called sumTest.js. Once you have done that, let's proceed to create a test.

```powershell
const assert = require('assert');
 const { sum } = require('../sum');
describe("sum", function() {
  it("adds numbers", function() {
       assert.equal(sum(2, 3), 5);
  });
});
```

# Run test

**Note**:- To run the test you must have set up a test script in package.json(as mentioned in the mocha's documentation):

```powershell
"scripts": {
  "test": "mocha"
}
```

After that just hit a simple command in your terminal:

```powershell
npm test
```

The result should be displayed like this in your terminal:

![test.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1660730969591/hV3K6Afz2.PNG align="left")

Well done! The test is passed. It means our function should be working as we intended.

### Furthermore, we also have hooks in Mocha

Mocha allows you to create test hooks. Hooks are essentially instructions that are set to execute before or after tests. They are beneficial for establishing prerequisites for tests or tidying up assets once tests are completed. These hooks provide a convenient way to prepare for tests and handle necessary cleanup tasks afterward.

* before()
    
* beforeEach()
    
* after()
    
* afterEach()
    

check-in official docs for more [here](https://mochajs.org/#hooks)

### That's it

![wait-hold-up.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1660731241229/sFAqPrybd.gif align="left")

When it comes to testing, we have a free open-source tool called ***Keploy*** that anyone can use without charge!

![kp.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660732648137/R4DPMBILc.png align="center")

[Keploy](https://github.com/keploy) is a no-code testing platform that generates tests from API calls. It converts API calls into test cases. Mocks are automatically generated with the actual request/responses.

Therefore, it is unnecessary to create lengthy test scenarios. Just integrate it into your project and that's it. It is available with different SDK support. Go check keploy docs for integrations and integrate them into your main project according to the desired SDK.

Also, keploy supports mocha integration for [typescript/javascript](https://github.com/keploy/typescript-sdk) SDK where we can see our test coverage. Kindly refer to its official docs [here](https://docs.keploy.io/). Please feel welcome to take a look at it!

#### So finally...that's it for this blog !!... Hope you all like and enjoy it!!

Some readings to get started with:

* https://javascript.info/testing-mocha
    
* https://mochajs.org/
    
* https://blog.logrocket.com/testing-node-js-mocha-chai/
    

Thank you for reading!