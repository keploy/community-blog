---
title: "Unit Testing in Python is way more convenient than you've thought"
seoTitle: "Unit Testing in Python"
seoDescription: "Discover the convenience of unit testing in Python with best practices and techniques for reliable and maintainable code"
datePublished: Mon Jun 03 2024 11:34:13 GMT+0000 (Coordinated Universal Time)
cuid: clwyw81dv000r09mcet780rsg
slug: unit-testing-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1715798596549/e2e5bf04-60f9-4e8d-a382-0dc878cf0417.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1715798708304/fd5c68e9-e012-4507-b68b-703e642de665.png
tags: software-development, python, testing, software-engineering, ci-cd

---

## Introduction

As software developers, we all write lots and lots of lines of code while building an application. But to ensure that each and every components work perfectly in the software, we really need to do some unit testing. This ensures proper functionality and reliable performance of our product. These testing of individual components is known as Unit Testing.

For the dynamic nature and the ease of writing tests alongside the code, Python can be a viable option for unit testing of our software. So, let's dive into the nitty-gritty of writing unit tests and explore the best practices and techniques to ensure our code's reliability and maintainability.  

![Let's go! - Pierce County Executive](https://blog.piercecountywa.gov/executive/files/2021/01/Lets-go.jpeg align="center")

## Why software needs to be unit tested?

Often it's found that during the early development phase, unit tests serve as a safety net by helping us catching bugs and regressions. By verifying the behavior of individual units, one can always able to identify and fix issues before they propagate throughout the codebase.

Also, well-written unit tests act as documentation for the code, providing examples of its expected behavior. When making changes or refactoring code, we can easily rely on the existing tests to ensure that modifications don't inadvertently break functionality.

## How to use Python for unit testing?

![How to Unit Test with Python - Mattermost](https://mattermost.com/wp-content/uploads/2022/04/02_Unit_Testing@2x.png align="center")

In python, we generally write unit tests using testing frameworks such as `unittest`. These tests validate specific behaviors of individual units, typically by asserting expected outcomes against actual results. The `unittest` module is included in the Python's standard library, which provides a framework for organizing and running unit tests and offers its own classes and methods for creating the test cases, running them, and reporting the results.

## How to write your first Python testcase?

For this case, we are considering a simple Flask application which is using MongoDB as the database. Now, we will be writing some APIs to interact with the database and we'll be testing them by writing unit testcases for them using the `unittest` library!!

### Let's first create our application

Let's create the file `app.py`. Here, we will connect out app with the database and write our API !!

First, let's import our necessary packages, and connect the MongoDB container named `task_manager` with our application:

```python
from flask import Flask, request, jsonify
from pymongo import MongoClient
from flask_cors import CORS
from bson import ObjectId

app = Flask(__name__)
cors = CORS(app, resources={r"/api/*": {"origins": "*"}})

client = MongoClient('mongodb://localhost:27017/task_manager')
db = client['task_manager']
collection = db['tasks']
```

Now, with the database being connected, let's write one API for the application:

```python
@app.route('/api/tasks', methods=['POST'])
def create_task():
    data = request.get_json()
    task = {
        'title': data['title'],
        'description': data['description']
    }
    result = collection.insert_one(task)
    return jsonify({'message': 'Task created successfully', 'id': str(result.inserted_id)}), 201
```

With our app being ready, now let's write the unit test case for this API we have just wrote!!

### Let's write the testcases

Let's create another file called `test_app.py` where we'll write the test case for our application. Now, let's first import the necessary libraries required for the testing purpose:

```python
import unittest
from app import app
from unittest.mock import patch, MagicMock
import json
from bson import ObjectId
```

Now, let's create our testing class and do the setup along with writing the testcase for our `POST` request API:

```python
class TestTaskManager(unittest.TestCase):
    # Unit test case for testing the Task Manager API.
    def setUp(self):
        """
        Set up the test client and mock the collection used in the app.
        This method runs before each test.
        """
        # Initialize the test client for the app
        self.app = app.test_client()
        self.app.testing = True

        # Create a mock for the collection used in the app
        self.collection_mock = MagicMock()

        # Patch the collection in the app with the mock
        self.patcher = patch('app.collection', self.collection_mock)
        self.patcher.start()

    def tearDown(self):
        """
        Stop the patcher after each test.
        This method runs after each test.
        """
        # Stop the patcher to clean up after tests
        self.patcher.stop()

    def test_create_task(self):
        """
        Test the task creation endpoint.
        """
        # Data to be sent in the POST request
        data = {'title': 'Test Task', 'description': 'This is a test task'}
        
        # Make a POST request to create a new task
        response = self.app.post('/api/tasks', json=data)
        
        # Assert that the response status code is 201 (Created)
        self.assertEqual(response.status_code, 201)
        
        # Assert that the response contains the success message
        self.assertIn(b'Task created successfully', response.data)
```

Now, that out test case has been written, it's time to put it on some test. So, we need to run the command `python3 test_app.py` in the terminal to get the result of the test,

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715809098489/ef4d2888-204a-4dda-ba8a-4ce7536d0b31.png align="center")

which look kind of like this!! And, as we can see that our test has passed successfully.

In case of any failure, we will get the result something like this, when the command is run in the terminal:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717412640981/d3889c07-9c95-42f6-b1f2-6aaeac704f33.png align="center")

This is could happen if there is any problem in the data that is being sent has some flaws or there is any problem with the address.

Now, if we had multiple APIs in our application, we would have to write more test cases to test each one of them!!

## How to check the test coverage?

Now, what if we want to check how much code does our written unit test cases cover!? Here comes Keploy, which helps us to easily check our test coverage in some simple steps.

![Keploy - Open Source Stubs and API Test Generator for Developer](https://keploy.io/_next/static/media/keploy.eb069ede.svg align="center")

First we need to install Keploy's Python SDK:

```bash
pip install keploy pytest
```

Next, we can create a test file for running Keploy's API tests and we can name the file `test_keploy.py` and the contents of the file will be as follows:

```python
from keploy import run, RunOptions

def test_keploy():
    try:
        options = RunOptions(delay=15, debug=False, port=0)
    except ValueError as e:
        print(e)
    run("python3 -m coverage run -p --data-file=.coverage.keploy python3 app.py", options)
```

We also need to create a `.coveragerc` file to ignore the coverage of the libraries that is calculated. The contents of the file will be as follows:

```bash
[run]
omit =
    /usr/*
sigterm = true
```

Now to run our unit test with Keploy, we can run the command given below:

```bash
python3 -m coverage run -p --data-file=.coverage.unit -m pytest -s test_keploy.py test_app.py
```

Now, to combine the coverage from the unit tests, and Keploy's API tests, and then generate the coverage report for the test run we can use these commands:

```bash
python3 -m coverage combine
python3 -m coverage report
```

## Best practices for writing test cases

The practices mentioned below are not exclusive to Python, but works for all kinds of unit test cases. But as we are discussing about unit test case writing here, so it's important to mention it here:

1. **Keep Tests Simple and Focused**
    
    While writing a unit test, we must focus on a single aspect of functionality, keeping the test cases as simple and easy to understand as possible.
    
2. **Use Descriptive Test Names**
    
    Clear and descriptive test names improve the readability and help other developers/maintainers to understand the purpose of each test case.
    
3. **Isolate Test Cases**
    
    We should avoid dependencies between test cases by isolating the units under test. And for that, we can use techniques such as mocking or dependency injection to replace external dependencies with test doubles.
    

## Common pitfalls to avoid while writing test cases

Now that we know about the best practices of writing unit test cases, let's focus on some common mistakes that we must avoid while writing the test cases. These includes:

1. **Testing Implementation Details**
    
    Always avoid testing the implementation details, because as far as I've seen, these tests can become prone to breaking when refactoring the code.
    
2. **Ignoring Edge Cases**
    
    Remember to ensure that our unit tests cover edge cases and boundary conditions to validate the robustness of our code under various problematic scenarios.
    

## Conclusion

Writing unit tests is a fundamental aspect of software development, which ensures the code reliability and maintainability. And by following best practices and leveraging advanced techniques, and also embracing tools like **Keploy**, developers can create robust test suites that validate their code's behavior under different conditions. And well, that's a wrap for now!! Hope you folks have enriched yourself today with lots of known or unknown concepts. I wish you a great day ahead and till then keep learning and keep exploring!!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715810624477/4f7bf965-9f83-4208-91e4-f09c2e78a977.png align="center")

---

## FAQs

1. ### **What is the purpose of unit testing?**
    
    Unit testing aims to validate the individual units or components of a software application to ensure that they behave as expected, enhancing code reliability and maintainability.
    
2. ### **How do I write my first unit test in Python?**
    
    To write your first unit test in Python, you'll need to set up a test environment, create a test case class that inherits from `unittest.TestCase`, write test methods within the class to validate specific behaviors, and then run your tests using a test runner.
    
3. ### **Which tools and libraries are available for unit testing in Python?**
    
    Python offers several tools and libraries for unit testing, including pytest, which simplifies test writing and execution, and nose2, an extension of Python's built-in unittest framework with additional features for test discovery and running.
    

---