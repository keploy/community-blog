---
title: "Python Testing with Pytest: Features & Best Practices"
seoTitle: "Python Testing with Pytest: Features & Best Practices"
seoDescription: "Pytest Explained: Step-by-Step Guide to Writing Better Python Unit Tests, Exploring Fixtures, Markers, Parametrize, and More."
datePublished: Fri Jan 17 2025 06:21:39 GMT+0000 (Coordinated Universal Time)
cuid: cm60dgaxq001e08iibawt3iye
slug: python-testing-with-pytest-features-best-practices
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1737094889224/ae8aeaad-ee3e-4059-8fa2-ceba0208f89a.png
tags: python, pytest, Python

---

The importance that we give to developing the application with the software engineering best practices, and principles, the equal importance and significance lies to the unit testing side of the codebase. This is where Pytest, a robust, scalable, feature-rich, unit testing framework for Python comes into picture. It’s being widely used by many open source teams and big organizations. Pytest is adaptable to all scenarios irrespective of the use case be it ML, LLMs, Networking, WebDev, etc.

## How to Setup PyTest?

[Pytest](https://docs.pytest.org/) is available as a Python package. We can install it using the `pip` command.

```bash
pip install -U pytest
```

Now to verify if pytest has been installed we can run the following command from CLI:

```bash
pytest --version
pytest 8.3.4
```

The pytest version may vary according to the installation. Otherwise you can import pytest and get the version as well. This can be done to verify the runtime version of pytest and it doesn’t interfere with the global installation in case.

## Writing Your First Unit Test

Now we can start with writing our first unit test. This function checks if the `greetings` matches the expected result. This seems naive because it’s pretty straightforward but this is the Hello world to understand pytest.

```python
# tests/test_hello.py
def test_hello_world():
    greetings = "Hello, Pytest!"
    assert greetings == "Hello, Pytest!"
```

The function starts with `test_`. The reason to have this convention is the pytest runs all the functions or filename starting with `test_`. We can also invoke a specific test using the path to the file, a specific function inside a file, class, methods inside a class, matching a regex, markers, and more.

To run the test all we have to do is run the command `pytest` or `pytest tests/test_hello.py` from the CLI.

```bash
pytest tests/test_hello.py
============================ test session starts ===============
platform darwin -- Python 3.9.6, pytest-8.3.4, pluggy-1.5.0
rootdir: ~/
collected 1 item                                                                                                                             

tests/test_hello.py [100%]

============================ 1 passed in 0.00s ================
```

### Understanding Test Output

Let’s decode the output:

* Initially, the test session starts.
    
* Then pytest detects the python version, pytest version, and pluggy version from the python environment it runs.
    
* Then it creates a .pytest\_cache directory, discussed later in the blog. Will be present if the tests have been run earlier.
    
* Then the root directory where the `tests/` folder exists.
    
* Config file which is the `pytest.ini` or any other config file will be using the necessary parameters while executing the tests. It will be present if there is a [configuration file](https://docs.pytest.org/en/stable/reference/customize.html#configuration-file-formats) present.
    
* The `collected` item refers to the total number of test cases that need to be run. In our case, it's just one.
    
* Then we execute the list of tests. \[100%\] is a progress indicator of how many tests have been executed rather than the status of the test.
    
* At the end, we have test results, which show how many tests passed, failed, skipped, xfail, etc.
    

This is the basic overview of the unit test setup and execution.

## Dissecting a Test: Key Components

For every test execution, we must understand what happens under the hood to write better unit tests. There are four key actions that we must be aware of while running a test.

### Arrange

Here we set up the environment to execute the tests. This involves database creation, preparing objects, establishing API connections, providing credentials, etc. This must be the first action item to be mindful of during the execution of a unit test.

### Act

This is the short form for action which means the event that triggers and changes the system. This can be anything like doing nothing, calling another unit test, or even performing a complex task. This modifies the state of the system and after successful completion will be verified.

### Assert

The Oxford meaning is to state a fact. Now we check the facts and the results of the Act. If they match then we can tell if our test has passed successfully or otherwise. We can judge the unit test based on the assertion(s).

### Cleanup

As always in any OOPs-based language, when the class object goes out of scope we delete the class object, and in the same way, we clear the test resources.  

> *At its core, the test is ultimately the act and assert steps, with the arrange step only providing the context. Behavior exists between act and assert.*

## Fixtures in Pytest

A fixture provides a context for the unit test(s). They are explicit, modular, and scalable. This can be any database, dataset, or a set of predefined values. This belongs to the arrange part of the unit test. A function becomes a fixture by using the `@pytest.fixture` decorator. We can invoke the parameters of the fixture through the arguments. We can use the fixtures throughout the test execution. Also, a fixture can be used to call another fixture as well. This is where the real power of fixtures comes into the picture.

```python
import pytest
from add import Add

# Arrange
@pytest.fixture
def test_add_values():
   return 2, 3

class TestAddFixture:
   def test_addition(self, test_add_values):
       # Act
       x, y = test_add_values
       result = Add.add(x, y)

       # Assert
       assert result == 5, "Addition result should be 5"
```

In spite of multiple parameters for fixtures like fixture\_function, scope, params, autouse, ids, and name one of the most important is the scope parameter. We can use fixtures with/without decorators.

### Scope of Fixtures

It defines the extent to which the fixture is available during the lifetime of a fixture. The default scope of a fixture is `function`. All available scopes are (in ascending order):

* function: Fixture is destroyed at the end of the test.
    
* class: Fixture is destroyed during teardown of the last test in the class.
    
* module: Fixture is destroyed during teardown of the last test in the module.
    
* package: Fixture is destroyed during the teardown of the last test in the package.
    
* session: Fixture is destroyed at the end of the test session.
    

A scope can be either static or dynamic. We can make a fixture scope dynamic by passing a Callable i.e. function.

## Using Marks to Categorize Tests

A mark is used for marking our tests. Just like we use markers with different colors each with its own significance in similar terms, mark means to signify the importance of a test case. If you know about the Robot framework, while defining the robot test case file there are tags for each test case. We can run the tests based on the tags. Here’s an example of unit test using the pytest mark feature:

We have an Add class that performs the addition function. Then we import this function and experiment with the mark feature.

```python
# add.py
class Add:
    @staticmethod
    def add(x: int, y: int) -> int:
        return x + y
```

The `setup_method` and `teardown_method` are special functions that are called during the arrange and teardown events of the test session. These are basic setup and clearance of the test environment.

```python
# tests/test_add_mark.py
import pytest
from add import Add

class TestAdd:
    # Arrange
    def setup_method(self):
        self.add = Add()

    @pytest.mark.skip(reason="Demonstrates skipping a test unconditionally.")
    def test_add_zero(self):
        # Act
        result = self.add.add(0, 0)
        # Assert
        assert result == 0, "Addition of zero(s) failed"

    @pytest.mark.skipif(reason="Skipping on specific condition", condition=(1 + 1 == 2))
    def test_add_positive_numbers(self):
        result = self.add.add(3, 5)
        assert result == 8, "Addition of positive numbers failed"

    @pytest.mark.xfail(
        reason="Known issue with negative numbers addition",
        raises=TypeError,
        strict=False,
    )
    def test_add_negative_numbers(self):
        result = self.add.add(-1, -1)
        assert result == -2, "Addition of negative numbers failed"

    @pytest.mark.filterwarnings("ignore:.*division.*:RuntimeWarning")
    def test_warning_filter(self):
        with pytest.warns(RuntimeWarning, match="division"):
            result = self.add.add(1 / 0, 1)
            assert result is not None, "Warning filter failed"

    @pytest.mark.addlargenumbers
    def test_add_large_numbers(self):
        result = self.add.add(10**6, 10**6)
        assert result == 2 * 10**6, "Addition of large numbers failed"

    def teardown_method(self):
        # Teardown
        del self.add
```

When we the execute the code for more clarity we will notice to the short summary information gives a clear picture of what the test suite execution yields. Here we are running basic tests on the `add` method wantedly passing, skipping, and failing a few test cases for displaying the features.

The various mark decorators are as follows:

* **skip**: Jump the test case with/without displaying a reason
    
* **skipif**: Jump the test case if it matches a certain criteria
    
* **xfail**: Mark the test case to expect a failure
    
* **filterwarnings**: Granular control on the warnings that need to be captured during the test execution.
    
* **custom**: Use custom metadata to mark a function and execute specific markers only
    

**Note**: While defining a custom marker ensure to add the marker to the configuration file like i.e. `pytest.ini` file or others. Here’s an example that needs to be added to the `pytest.ini` file.

```bash
markers =
    addlargenumbers: Custom test marker to verify behavior for large numbers
```

## Parametrized Testing for Better Coverage

It can be sometimes difficult to write tests for each combination of inputs. If the test codebase grows eventually then it becomes difficult to manage. So we have a parametrize feature where we provide the parameters and test the input and match it against the expected output.  
Here’s an example parametrize feature that takes in multiple different inputs and executes as each test case.

```python
# tests/test_add_parametrize.py
import pytest
from add import Add


@pytest.mark.parametrize(
    "x, y, expected", [
        (1, 2, 3),
        ("a", "b", "ab"),
        (-3, 3, 0),
        (0, 0, 0),
        (1, 2, -3),
        ([1], [2], [1, 2]),
    ]
)
class TestAddParametrize:
    def setup_method(self):
        self.add = Add()

    def test_add(self, x, y, expected):
        result = self.add.add(x, y)
        assert result == expected
        
    def teardown_method(self):
        del self.add
```

While executing the multiple cases, one of them will fail (***intentionally***) because the behaviour doesn’t match the expected. We get the values that were used and then what failed with the error. The final line tells the summary of the test suite execution.

### Conftest : Centralized Fixture Management

Consider a scenario where instead of a single test file we have a complex system where classes are nested and tests are widespread then we would need to add a lot of fixtures which can be repetitive too. This makes the codebase difficult to maintain in the longer run. That’s where the `conftest.py` comes to the rescue. This file contains one or more fixtures where the current test execution takes this fixture while passing as an argument.

Here’s an example unit test that takes in fixture from the `conftest.py` and executes the tests:

```python
# tests/conftest.py
import pytest

@pytest.fixture(scope="function")
def add_data():
    return [
        (1, 2, 3),
        (5, 7, 12),
        (10, -2, 8),
        (0, 0, 0),
    ]
```

There’s much more features that can be incorporated into the `conftest.py` file. Few of them are:

* Fixtures
    
* Plugin loading
    
* Hooks
    
* Test root path
    

Scope of the `conftest.py` in case there are multiple ones:

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXegfj9ID2-7Nqg5n50zBI3htIwveULI_q9fVBwhQb7DpRmVeKCSlCuxQXl6RP_msY_6795PJUkSYT0V2G80OkeWZhvS9IN72czEAUx3vz8ZvwUYLrmGQDPoAXHyJCt4rS3BCHZPvw?key=aPObjI-wsDBq8gE4OjaZi-nh align="left")

## Optimizing Configuration with Pytest.ini

We can configure a test execution in multiple ways but `pytest.ini` takes the top priority over all other configurations. This file can have contents or be empty as file. This has a format closer to TOML but not the same. An example structure of ini file:

```bash
[pytest]
minversion = 6.0
addopts = -ra -q
testpaths =
    tests
markers =
    addlargenumbers: Custom test marker to verify behavior for large numbers
```

This can be made more extensive too. We can see the `conftest.ini` config file being taken up while running the test.

## CLI Features and Arguments

This is a beast feature of pytest. We can configure a plenty of things from the CLI. We can see the help function from the CLI itself and understand its usage.

```bash
pytest -h
usage: pytest [options] [file_or_dir] [file_or_dir] [...]
```

The standout flags are as follows:

* –pdb: Enables debugger at the test failure. Very much similar to [gdb](https://sourceware.org/gdb/).
    
* \-m: Execute the unit tests belonging to that marker
    
* \--show-capture: Controls the standard output verbosity(stdout/stderr/log). Default is enabled and captures all of them
    
* \-v: Controls the output verbosity of test execution output
    
* \-q: Decreases the output verbosity
    

These are the most common ones but you can find the list through the docs or running the `help` command.

## Enhancing Tests with Plugins

As the name suggests, there are a bunch of pytest plugins that are use case specific. Though these are not from the official pytest plugins but are maintained by the external community. This curated list consists of plugins from the pypi repository starting with `pytest-` or `pytest_`. We can see if it’s maintained actively with the status of the plugin. We can call the plugins from the `pytest.ini` file.

## AI Pytest : Using AI for Testing

Trending AI tools like ChatGPT, Codium, and others have made development easy by help with generating test cases. These tools provide quick test drafts, reduce repetitive tasks, and improve developer productivity. However though, the snippets which they provide are very generic in nature and usually results in challenges:

* **Generic Nature**: AI tools may create broad or shallow tests that lack application-specific coverage.
    
* **Contextual Gaps**: Understanding intricate business logic or domain-specific nuances can be tricky for AI.
    
* **Maintenance**: Test cases may require fine-tuning to align with updates in the application logic.
    

## **Pytest with Keploy : AI Agent for Testing**

Keploy offers a unique approach by auto-generating test cases based on actual application code semantics, which ensures:

* **Context-Specific Tests**: Generated tests based on actually logic of the function.
    
* **Enhanced Coverage**: Captures edge cases and complex workflows effortlessly.
    
* **Reduced Manual Effort**: Automates test creation, reducing dependency on AI-generated drafts alone.
    

By integrating AI tools with Keploy, developers can harness the strengths of both worlds - AI's speed and Keploy's precision - to create robust, high-coverage test suites with Pytest.

## Conclusion

To conclude Pytest is powerful, versatile, adaptable, scalable, and whatnot. It's easier to be integrated into your existing workflow. There are endless experiments, features, and development that can be performed using pytest. Though it’s not just used for unit testing but is capable of performing integration testing by using the right practices. Feel free to experiment with this amazing framework.

## FAQs

### **What are fixtures, and how do they help in testing?**

Fixtures are reusable pieces of code that set up the required context for tests. They handle tasks like initializing objects, setting up databases, or preparing mock data, making your tests modular and cleaner.

### **What is Pytest, and why should I use it?**

Pytest is a Python testing framework known for its simplicity, scalability, and rich feature set. It's widely used in unit, integration, and even functional testing due to its flexibility and powerful tools like fixtures and markers.

### **How can AI tools like ChatGPT and Codium help in writing test cases for Pytest?**

AI tools can assist by generating initial test drafts based on code snippets, reducing repetitive efforts, and offering suggestions for common scenarios. They speed up the test-writing process, especially for standard or straightforward use cases.

### **What are the limitations of using AI tools for test case generation?**

AI-generated tests often lack depth and specificity, leading to generic test cases. They may fail to capture complex business logic, domain-specific scenarios, or edge cases, requiring manual intervention for refinement.

### **Can I use Keploy with Pytest directly?**

Yes, Keploy integrates seamlessly with Python applications and can generate test cases compatible with Pytest, allowing developers to run and maintain robust test suites effortlessly.