---
title: "Monitor API Calls in Chrome and Validate Flask APIs"
seoTitle: "Track Website API Calls in Chrome & Validate Flask APIs with Python"
seoDescription: "Discover how to monitor API calls in Chrome, reproduce them with Python, and validate Flask REST API inputs like a pro."
datePublished: Thu Aug 21 2025 05:42:41 GMT+0000 (Coordinated Universal Time)
cuid: cmekz66mw001902jv8x2s7pmc
slug: monitor-api-calls-in-chrome-and-validate-flask-apis
canonical: https://keploy.io/blog/community/monitor-api-calls-chrome-validate-flask-apis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753075943872/33f58374-2a04-4b2d-9517-4a14f8d91757.png
tags: python, apis, monitoring, rest-api, keploy

---

You have probably seen pages where fresh information loads in without a page reload, or some forms that submit without an apparent refresh. What is happening here is that API calls are being made to send and receive information in background.

API calls generate seamless and responsive application experiences. In this introductory tutorial, you will learn to examine an API call in the Chrome DevTools; replay it in Python, and sanitise the Flask REST API data for safe and organised input into the database. The processes presented are applicable for backend developers and QA engineers testing reliable systems.

## How to Track a Website’s API Calls in Chrome (and their Simulation using Python)

Have you ever viewed a page that loads results instantly with no need for a page reload? Or form submission of a login form with a sexy spinner and no reload? That's under-the-hood JavaScript and AJAX wizardry, making API calls to send and get data. Those under-the-hood calls are what drive dynamic websites now.

But the icing on the cake is - you can inspect these calls in Chrome DevTools and even export them out using Python. It's a convenient thing to know if you're learning web scraping, API testing, or debugging your own front-end/back-end configuration.

So let's walk through it.

**Step 1: Inspect API Calls With Chrome DevTools To get under the hood of a webpage**

* Open Chrome and visit any dynamic web page (i.e., a product search, login, or dashboard).
    
* Press Ctrl + Shift + I (or Cmd + Option + I on Mac) to turn Chrome DevTools on and off.
    

![Chrome Devtool](https://cdn.hashnode.com/res/hashnode/image/upload/v1754900375973/9d633450-b078-4faa-8050-22d411b53722.png align="center")

* Go to the **Network tab** - this will display all of the requests your browser is making to the outside world.
    

![Chrome Devtool Network Tab](https://cdn.hashnode.com/res/hashnode/image/upload/v1749796579739/2a8ecb72-73a2-45e1-8973-2a18acaec58b.png align="center")

* Filter by **XHR** or **Fetch** - but only on the API requests, not static assets like images and CSS.
    

![Chrome Devtool Fetch/XHR](https://cdn.hashnode.com/res/hashnode/image/upload/v1749801723997/6c7be720-1367-4f03-8663-31f57cad55e6.png align="center")

* You should be able to view a list of network requests at this point. Each item in a row is an API request made by the browser to a backend service
    

![Chrome Devtool Row](https://cdn.hashnode.com/res/hashnode/image/upload/v1749801981900/0289c609-4a52-478e-958c-a022132faf07.png align="left")

* Right-click anywhere within a row for more detailed information:
    

![Chrome Devtool Network tab Header](https://cdn.hashnode.com/res/hashnode/image/upload/v1749802516802/56291293-ce68-4576-a2b0-ffdaa9c7a444.png align="center")

* **Request URL** – target API endpoint
    
* **Method** – GET, POST, PUT, DELETE, etc.
    
* **Headers** – important details such as tokens for authentication or content type
    
* **Query Parameters** – values in the URL
    
* **Payload/Request Body** – data transmitted in POST or PUT
    
* **Response** – data retrieved from the server
    

**Pro Tip:** Right-click on any request → Copy → Copy as **cURL**

![Chrome Devtool Copy as a curl](https://cdn.hashnode.com/res/hashnode/image/upload/v1749820024041/7ba23f68-fa4d-4f72-86bd-8c1c43703d8f.png align="center")

* This will leave you with a command which you can execute in advance so that you are able to replay the request in your command line, or else you are able to translate it into Python
    

**Step 2: Replay the API Call in Python**

* We just recorded that API call, so now let's replay it. With Python's requests library, we can replay exactly the same call site made - great for testing and automation or just a bit of learning.
    

**Example: GitHub Repository Info**

When you go to [Keploy.io](http://Keploy.io), it makes a behind-the-scenes GET to:

```powershell
https://api.github.com/repos/keploy/keploy
```

* This is a GET of Keploy's GitHub repository metrics (i.e., stars, forks, etc.)
    
* We will now execute that very same request with Python.
    

### Python Code to Replay the API Request

Here's some clean and neat Python code using the `requests` library:

```python
import requests

url = "https://api.github.com/repos/keploy/keploy"

headers = {
    "Content-Type": "application/json",
    "User-Agent": "Mozilla/5.0"
}

response = requests.get(url)

print(" Status Code:", response.status_code)

if response.status_code == 200:
    data = response.json()
    print(" Repository:", data.get("full_name"))
    print(" Stars:", data.get("stargazers_count"))
else:
    print(" Error Response:", response.text)
```

#### Output (Sample)

```powershell
Status Code: 200

Repository: keploy/keploy

Stars: 1200
```

![Output of api request](https://cdn.hashnode.com/res/hashnode/image/upload/v1750144595214/174c62e4-340a-42eb-96e2-768e7deed762.png align="left")

### What This Does:

* Sends a simple GET request to GitHub's API
    
* Parses the JSON response if the request was successful
    
* Prints handy repository information such as
    
* `full_name`
    
* `stargazers_count`
    

This code replicates the same exact API action of the site onto an API testable Python environment

**Bonus: Why Should You Care About API Call Monitoring? It isn't "hacking curiosity" - it's handy and useful. Here's why you should care:**

### **1\. Take a cue from other people's apps**

Need to know how your favourite apps retrieve data, authenticate a user, or construct requests? Just check their API calls.

### **2\. Scrape smarter**

Rather than scraping HTML, take advantage of clean, structured JSON responses. Smarter, faster, stronger.

### **3\. Test like a pro**

QA engineers use this hack to test API returns, validate error handling, or test edge cases.

### **4\. Repair your own apps**

If your frontend is broken, make sure the API which is giving you the correct thing. You'll debug a lot quicker.

### **5\. SPA behaviour ninja**

All single-page Application (React, Vue, Angular) reduces to these dynamic API calls. When you get them, you've got the entire stack. That's just the tip of the iceberg - now you can tap into a site's underlying dialogue and replay it yourself with Python.

Then we'll discuss sanitising incoming data to your own Flask-based APIs so only clean data can flow through.

## [Keploy API Test Recorder](https://chromewebstore.google.com/detail/keploy-api-test-recorder/ohcclfkaidblnjnggclkiecgkpgldihe) (Chrome Extension)

### Record, Replay, and Create API Tests—Quicker Than You Can Nuke Leftovers

Seriously, all that migraine-inducing manual API test writing. Keploy API Test Recorder? This handy little Chrome buddy just records all the API calls your browser spits out, allows you to hit replay like a DJ, and - ta-da - test cases. All in Chrome. No wizard hat required by DevOps. Magic? No, more like "finally, someone did this."

![Keploy API Test Recorder](https://cdn.hashnode.com/res/hashnode/image/upload/v1753260645459/1306dbaf-eddf-4c27-b7f4-aa8e3d84480d.png align="center")

### What the Extension Does

#### **Browser-Side Traffic Capture**

Saves all XHR/Fetch API calls whenever you run your app.

#### **Instant Replay Formats**

Save recordings in your preferred format: cURL, JSON, or native Keploy YAML.

#### **URL Filtering & Debugging**

Trim recordings to endpoints and auto-complete missing request/response pairs.

#### **One-Click Test Generation**

Send traffic recordings to Keploy Console and receive ready-to-exec test cases with assertions.

### Installation Guide

* **Open the Chrome Web Store Listing**
    
* Click **Add to Chrome → Add Extension**
    

After installation, pin the extension for quick access:

> Go to Extensions (⋮) → Click **Pin** next to **Keploy API Test Recorder**

### Quick Start

1. Log in using same email as app.keploy.io
    
2. Click **Record API Calls**
    
3. Open in a new tab, use your app as usual - sign up, add items to cart, etc.
    

#### Inspect the live counters:

* **Captured calls** – count of captured API calls
    
* **Complete req/resp** – request & response fully captured
    
* If lacking expected pairs, click **Debug** to automatically fix
    

### Debug Example:

```powershell
yamlCopyEditDEBUG SUMMARY:
Total calls: 33
Complete (status+body): 2
Incomplete: 31
Has status but no body: 13
Has body but no status: 0
Flagged as complete: 30
Records repaired: 15
```

### Press Generate Tests

* Your browser will be redirected to app.keploy.io
    
* A test suite with executable test cases will be generated automatically
    

#### Select Export Format:

* **cURL** – shareable command-line snippets
    
* **Keploy YAML** – executable directly with Keploy
    
* **JSON** – for custom tooling
    

Press **Export Data** to **Download** or **Copy to Clipboard!**

[Explore Keploy Extension](https://keploy.io/docs/running-keploy/api-testing-chrome-extension/)

## Python Flask REST API Validation – Inputs to Safe APIs.

Okay, now reverse that..

You now have a Flask REST API - yay!

But hold on - how do you validate input to your API? How do you ensure that

* The client supplies all the fields you need?
    
* The input is in good health?
    
* Isn't someone attempting to conceal malicious or malformed input?
    
    The answer is in one simple habit: **validation.**
    

> **Pro Tip**  
> Flask will not validate for you if the box isn't there - you're on your own. Good luck to you, you have a few different means of validating incoming data from simple paper checks pushed through quickly to third-party full-featured libraries such as Voluptuous. On with it.

### Option 1: Simple Manual Validation (Simple but Scales Horribly)

Take a look at the old-fashioned DIY solution.

**Here** is an example of a **basic** POST endpoint for **signing** **up a user**:

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route('/api/signup', methods=['POST'])
def signup():
    data = request.get_json()

    if not data.get("username") or not data.get("password"):
        return jsonify({"error": "username and password required"}), 400

    if len(data.get("password")) < 8:
        return jsonify({"error": "Password must be at least 8 characters"}), 400

    return jsonify({"message": "Signup successful!"}), 200
```

**Pros:**

* **Simple,** and gets the job done
    
* No libraries needed
    

**Cons**

* Code **will become very** cluttered with **items** **we** **don't** **use**
    
* **It will become very complex,** or **impossible to** maintain when **working** with deeply nested JSON objects
    
* **It won**'t type validate **(by** **default)**
    
* **Fine** **for** **learning** and small projects, but **when** **it** **counts,** **there** **is** **a** **better** **way.**
    

### Option 2: **Using** Pydantic for Declarative and Clean Validation

**Now** **we** **present** **the** solution **I talked about,** Pydantic. Pydantic is a data validation and configuration management library which offers a clean, well-organized means of retaining input validation.

**Step 1: Install Pydantic**

```python
pip install pydantic
```

**Step 2: Define Schema and Use It**

```python
from flask import Flask, request, jsonify
from pydantic import BaseModel, EmailStr, ValidationError

app = Flask(__name__)

class User(BaseModel):
    username: str
    email: EmailStr
    password: str

@app.route('/api/register', methods=['POST'])
def register():
    try:
        user = User(**request.get_json())
        return jsonify({"msg": "User data is valid", "user": user.dict()}), 200
    except ValidationError as e:
        return jsonify({"errors": e.errors()}), 422
```

**Why This Rocks:**

* Ensures type safety (e.g., proper email format, string vs int)
    
* Generates error messages automatically
    
* Gets validation modular and reusable
    
* Your code remains clean, readable, and DRY
    
    **Need more control? Pydantic also supports:**
    
* Regex constraints
    
* Custom validators
    
* Nested models
    

Perfect for production-level applications!

### **Option 3:** Validating Query Parameters (Not Just JSON!)

Sometimes you’re handling query params, like /api/products?limit=10.

Here's how to parse and validate them safely :

```python
@app.route('/api/products', methods=['GET'])
def get_products():
    try:
        limit = int(request.args.get("limit", 10))
        if limit <= 0:
            raise ValueError("Limit must be > 0")
        return jsonify({"msg": f"Returning {limit} products"})
    except ValueError as e:
        return jsonify({"error": str(e)}), 400
```

> **Pro Tip**  
> Never trust user input - a dirty value will destroy your backend or be unsafe. Sanitize query parameters everywhere!

### Tools You Should Know About (for Testing and Beyond)

The following is something that every Flask API developer should know:

| Tool | What It's Used For |
| --- | --- |
| **Postman** | Manual API testing, fast and graphical |
| **Pytest** | Python unit and functional tests |
| **Keploy** | Test cases directly from real traffic |
| **Flasgger / Swagger UI** | Auto document Flask APIs |
| **Flask-RESTX** | Enhanced routing, request parsing, and validation |

All these assist you in **debugging**, **documenting**, and testing your APIs better - less bugs and neater coding.

### Real-Life Scenario: E-commerce

You're creating a Flask backend for an online store. Your frontend engineers are invoking your API to:

* Display lists of products
    
* Add to cart
    
* Process payment and checkout
    

Something's wrong in prod. What do you do?

* Inspect API frontend calls via Chrome DevTools.
    
* Script the calls out manually in Python with requests for hand testing.
    
* Implement some validation rules in Flask so bad data won't crash your system.
    

**Boom** - You just prevented a production outage, you released a bug into the wild, and you froze out your API - all thanks to having skills this far.

**Final Thoughts**  
Validation is dull, but it's probably the most significant aspect of designing an API. Validation hs your backend to be:

* Secure
    
* Reliable
    
* Developer-friendly
    

And with the likes of Pydantic and Postman libraries, it's a breeze to write your API - at least, for you, and your consumers.

## Killer API Blogs You Seriously Need to Bookmark

### 1\. [REST APIs in Python, From Scratch](https://keploy.io/blog/community/introduction-to-rest-api-in-python)

A REST APIs guide for Python beginners. Get endpoints, HTTP verbs, and the requests library, without fluff.

### 2\. [Waterfall API: A Deep Dive](https://keploy.io/blog/community/waterfall-api-a-comprehensive-guide)

Explain how a API call can trigger others and why "waterfall" flows needs to be managed by developers.

### 3\. [Make Your API Testing Faster: Automated Test Case Generation](https://keploy.io/blog/community/test-case-generation-for-faster-api-testing)

Show how AI and automation tools can generate API test cases to save time and get rid of repetitive manual testing.

### 4\. [20 Rest Assured Alternatives That Are Not Bad](https://keploy.io/blog/community/rest-assured-alternatives-for-api-testing)

A collection of API testing tools like Postman, Karate, Hoppscotch with notes about what each is good at.

## Conclusion

Modern web applications rely on APIs to function are our those APIs when you learn how to capture and replay API calls using Python, it will benefit your debugging, testing and reverse engineering skills which brings you closer to understanding how the web really works.

Building APIs is not only about making APIs work, making APIs safe and robust is equally important. Performing input validation with Flask and providing frameworks like Pydantic or Marshmallow to solidify bad data doesn't break your application. In the end it is going to be validation that changes working code into production quality code.

## FAQs

### Q1. How can I observe a website's API calls in Chrome?

Open DevTools (F12 or Inspect), select the Network tab, select the filter by XHR/Fetch, and refresh the page. You'll see all API requests, URLs, methods, and responses.

### Q2. Want to play with website APIs using Python?

You can use the requests library to do GET or POST calls as if you are a browser. You just need to include headers, or cookies, or tokens so the API doesn't deny your call.

### Q3. How does Keploy assist with API testing?

Keploy automatically records real API calls and turns them into test cases, which saves time and gives you consistent and reliable CI/CD tests that are being tested with production like data.

### Q4. Can I automate testing my Flask APIs?

Absolutely! You can do it manually in Postman, or automated using Pytest. Keploy goes a step further by creating tests from real traffic, so there is accuracy in the tests.

### Q5. Why should I even validate my inputs?

If you don't validate your inputs then your app can be manipulated to accept bad or unsafe data. Validating your API's prevents unsafe or inconsistent apps from going out into the wild. It also saves you from random breakage.