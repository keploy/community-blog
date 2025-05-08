---
title: "How to Run Pytest Program??"
seoTitle: "Getting Started with Pytest"
seoDescription: "Learn to run Python tests with Pytest, covering installation, writing tests, and exploring automated testing tools"
datePublished: Thu May 08 2025 04:48:46 GMT+0000 (Coordinated Universal Time)
cuid: cmaew0ern000308l8b8f10mkn
slug: how-to-run-pytest-program
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1746679494893/95d1f8f0-db13-4aea-bd57-e50cbff2c821.webp
tags: python, pytest, python-testing, python-application-testing

---

When you're building something in Python—whether it's a personal project, an API, or a startup idea—one thing is certain: bugs happen. And while debugging can be fun (sometimes), wouldn’t it be better to catch issues before they cause problems?

That’s where testing comes in. In today’s blog, we’ll explore how to test and run your Python applications using Pytest, one of the most popular and beginner-friendly testing tools out there. We’ll also walk through some examples to show you exactly how to write and execute tests using Pytest.

Let’s dive in!

## How to Test Applications in Python?

Testing is all about making sure your code does what it’s supposed to. In Python, testing generally falls into a few categories:

* **Unit Testing**: Testing individual functions or methods.
    
* **Integration Testing**: Making sure different parts of your app work well together (e.g., API + DB).
    
* **Functional Testing(E2E)**: Testing end-to-end flows, like a user login.
    

Wait, you can also test your Python application manually, but it will take a lot of your time. And what if your application has more files and functions? You can't test it manually, so that’s why every language has frameworks or libraries to automate the process. One such framework is Pytest for Python.

While Python also has a built-in unittest module, most developers prefer Pytest for its simplicity and power.

Enough theory you now know why we are testing and why we prefer Pytest over Unittest, so let’s see some code.

## What is Pytest??

Pytest is a testing framework for Python that makes it easy to write small, scalable tests. It offers a new approach to unit testing, which is less complicated and more fun

## How to Install Pytest

**Super simple. Just run:**

```python
pip install -U pytest
```

Note: Make sure you're in a virtual environment

**To verify it’s installed:**

```python
pytest --version
```

**Here is how the Output will look like:**

![How to install pytest](https://cdn.hashnode.com/res/hashnode/image/upload/v1745550715474/f026d647-8ddd-411d-befb-679121b6af21.png align="center")

## Writing Our First Pytest Test

Let’s write a basic function and test it using Pytest.

**Step 1: Create an file named calculator.py**

```python
def add(a, b):
    return a + b
```

**Step 2: Create a test file named test\_calculator.py**

```python
from calculator import add

def test_add():
    assert add(2, 3) == 5
```

**Step 3: How to Run the pytest? Use the below command**

```python
pytest
```

Pytest will discover any files starting with test\_ and run them automatically. No need for classes or boilerplate code.

**Output of our first example**

![passed test cases](https://cdn.hashnode.com/res/hashnode/image/upload/v1745551323858/f69baf59-8784-476d-8074-e96645be0330.png align="center")

**Let’s make some changes in the test\_calculator.py:**

```python
from calculator import add

def test_add():
    assert add(2, 3) == 6
```

Even 5-year-old kids know 2 + 3 = 5, but let’s test this with Pytest and see what output we’re getting:

**Output**:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1745551688250/56aa408c-bfca-4253-b1b6-7cc556ab69be.png align="center")

Guess what? Our test failed because in the logic we are adding two numbers, but in the test we’re asserting 2 + 3 = 6 , so it failed.

## Let’s see an another simple pytest Example:

Example 2: Testing a String Function

**Let’s test if a string is a palindrome.**

```python
def is_palindrome(word):
    return word == word[::-1]
```

Let’s write pytest for the above function

```python
def test_is_palindrome():
    assert is_palindrome("madam") is True
    assert is_palindrome("banana") is True
```

Let’s see the Output:

**By the way if you want to run the specific Test file you can use:**

```python
pytest <file_name.py>
```

![How to run specific pytest file](https://cdn.hashnode.com/res/hashnode/image/upload/v1745552571542/2dac40be-c06c-4d66-ba4a-4900123aeb3d.png align="center")

You can see from the output, banana is not the palindrome so the test failed.

## **Advantages of Pytest**

* Simple and readable syntax.
    
* No boilerplate—just write tests as functions.
    
* Rich ecosystem (plugins, fixtures, parametrize).
    
* Great for both small and large projects.
    
* Easy to integrate with CI tools.
    

## **Tired of Writing Code for Tests? Let’s See How** [**Keploy**](https://keploy.io/) **Helps You**

Wait, even though we have a lot of advantages with Pytest, there’s still one big problem: we have to manually write tests for the application. And let’s be real we’re busy shipping features.

Don’t get misled by the above example. Those were just very simple programs I wrote to help beginners understand how Pytest works. But in reality, it’s tough to write a test case for each and every function.

That’s exactly where Keploy comes in.

[Keploy](https://keploy.io/) is a **zero-code testing platform** it automatically writes unit test cases for you by analyzing your source code. And no, I know what you’re thinking—it’s not just another ChatGPT wrapper! Keploy’s unit testing is completely different from other providers.

You can use Keploy’s unit testing product via the **VSCode extension**, or try the **Keploy PR Agent**, which automatically writes unit test cases whenever someone raises a PR.

Just remember, we’re in 2025, not 2018 or 2019. So, let’s start testing like it’s 2025.

**What are you waiting for? Just give it a try!**

Links to VScode Extension: [https://marketplace.visualstudio.com/items?itemName=Keploy.keployio](https://marketplace.visualstudio.com/items?itemName=Keploy.keployio)

Links to Github PR agent: [https://github.com/apps/keploy](https://github.com/apps/keploy)

## Conclusion

Testing doesn’t have to be hard. With Pytest, you get a simple, readable, and powerful framework that grows with your project.

In today’s blog, we’ve seen how to run a Pytest program. Wait those were just very basic programs! There are many more things to explore. Pytest has a variety of assert statements readily available in the docs you should definitely try those out too.

So, go ahead write your first pytest today and understand how to run it. You’ll thank yourself later when you catch a bug before it hits production.

If you want to learn more about the Python testing with pytest, I highly recommend checking out this [blog](https://keploy.io/blog/community/python-testing-with-pytest-features-best-practices?trk=article-ssr-frontend-pulse_little-text-block)

Don’t forget to try [Keploy](https://keploy.io/) and if you found Keploy useful, give it a star. Not in the sky 🌟—but on the repo! [https://github.com/keploy/keploy](https://github.com/keploy/keploy)

## FAQ’s

1. ### Is Pytest only for unit testing?
    
    Nope! Pytest can also be used for integration and functional testing. Just use fixtures and mocks to set up your dependencies.
    
2. ### Do I need to write tests manually with Keploy?
    
    Nope! With Keploy Unit testing agent even you dont need to write a prompt also. Just install it in your VScode or Github and let see keploy in action.
    
3. ### Can I use Pytest in a CI pipeline?
    
    Yes! Pytest works great with GitHub Actions, GitLab CI, Jenkins, and more. Just add a step like pytest in your pipeline.
    
4. ### Pytest vs Unittest — Which one is better?
    
    To be honest, Both have their pros, but most developers prefer Pytest for its cleaner syntax, less boilerplate, and powerful features like fixtures and plugins.  
    If you’re starting fresh, go with Pytest. But if you're maintaining legacy code that already uses unittest, it's still a solid choice.
    
5. ### Can I use Pytest with Django or Flask apps?
    
    Absolutely! Pytest works great with both Django and Flask. In fact, many devs prefer it over the built-in Django test runner because of its flexibility and powerful fixture system. There are even plugins like pytest-django and pytest-flask that make integration seamless.