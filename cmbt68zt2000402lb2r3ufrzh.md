---
title: "How to get your ChatGPT API key"
seoTitle: "How to Get Your ChatGPT API Key: Step-by-Step Guide"
seoDescription: "Learn how to obtain your ChatGPT API key and integrate it into your applications with this step-by-step guide."
datePublished: Thu Jun 12 2025 09:23:52 GMT+0000 (Coordinated Universal Time)
cuid: cmbt68zt2000402lb2r3ufrzh
slug: how-to-get-your-chatgpt-api-key
canonical: https://keploy.io/blog/community/how-to-get-your-chatgpt-api-key
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1748203925433/9caba3ae-94fb-4c84-80ea-b7746da0483c.png
tags: gpt-3, openai, generative-ai, chatgpt, chatgptapi, gpt-4, openai-api, gpt-4o

---

These days, even a 6th-grade student knows about [ChatGPT,](https://keploy.io/blog/community/testing-with-chatgpt-epic-wins-and-fails) right? I’m sure most of you reading this blog have tried ChatGPT at some point maybe to help with an assignment, generate some content, or just to play around. It’s truly a game changing innovation that’s helping people in so many ways.

But here’s the cool part: instead of only using ChatGPT through the browser, did you know you can also interact with it directly through an API?

Yes OpenAI offers an API that lets you integrate ChatGPT into your own apps, tools, and projects. Before you can start building the next AI-powered side hustle or your hackathon-winning project, you’ll need one important thing: **An** **API** **key**.

In this blog, we’ll walk through how to get your ChatGPT API key from the OpenAI platform. Whether you’re a student, a hobbyist, or a developer just starting out, don’t worry it’s super easy to follow.

## First, What’s an API Key?

An API key is like a password for applications. It helps a platform (like OpenAI) identify who you are and what you’re allowed to do. When using the ChatGPT API, you include this key in your code so OpenAI knows you're authorized to use their services.

And it’s not just OpenAI almost any service that offers an API requires a key. Without it, you’ll usually get an error like 403 Forbidden, which means “you’re not allowed in.”

**Note:** In this blog, you might see me use both “OpenAI” and “ChatGPT.” Don’t get confused OpenAI is the company behind ChatGPT, and ChatGPT is the product. Also, ChatGPT has different versions or models, like **GPT-4o**, **GPT-3.5**, and **o4-mini**. You’ll see these mentioned when choosing which model to use via the API.

## What’s the ChatGPT API Anyway?

The ChatGPT [API](https://keploy.io/blog/community/ai-powered-test-automation) allows developers to plug ChatGPT’s intelligence into their own apps, websites, or tools. You can use it to:

* Build AI chatbots
    
* Create automated writing tools
    
* Power virtual assistants
    
* Add smart replies to your app
    
* Or just experiment with fun and creative ideas
    

It works by sending a prompt (a message from your app or user), and getting a response back—just like chatting with ChatGPT in your browser.

## How Does the ChatGPT API Work?

Here’s a quick example in [Python](https://keploy.io/blog/community/difference-between-pytest-and-unittest):

```python
from openai import OpenAI
client = OpenAI()

response = client.responses.create(
    model="gpt-4.1",
    input="Write a one-sentence bedtime story about a unicorn."
)

print(response.output_text)
```

It’s that simple. You send a message, and it replies.

For this demo, I’ve used [Python](https://keploy.io/blog/community/how-to-run-pytest-program) but you can use any programming language you’re comfortable with. OpenAI provides support for multiple languages to interact with their models through SDKs.

But wait first you need that key. Let’s walk through how to get it.

## How to Get Your ChatGPT API Key (Step-by-Step)

### 1\. Create an OpenAI Account

First go the OpenAI platform then You can use your email or sign in with Google or Microsoft.

### 2\. Verify Your Account

After signing up, OpenAI may ask you to verify your phone number. This helps prevent spam or bots from abusing the system.

### 3\. Log In to Your Dashboard

Once verified, go to platform.openai.com and log in.  
You’ll land on the developer dashboard where you can manage your account, usage, and keys.

![OpenAI Dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1748202072975/34192f7e-2fc0-492d-88c7-f7282ffe0ece.png align="center")

Note: If OpenAI asking you to create an organization feel free to create one. You can give any name of your preference.

### 4\. **At the top, select the 'Projects' section.**

Once you've selected your project, please click on the 'Manage Projects' option to proceed

![OpenAI Default Project](https://cdn.hashnode.com/res/hashnode/image/upload/v1748398303630/e6027763-81f7-4798-851e-e30430f2b8b6.png align="center")

In the left menu click API Keys

![OpenAI API Key section](https://cdn.hashnode.com/res/hashnode/image/upload/v1748203512377/4bd307ac-a629-4329-bcad-3b5bfe0388b3.png align="center")

Wait! If you feel you are so bored of the above step, in just one single step you can get the API key. Once you've landed on platform.openai.com, please select the Dashboard section. From there, you can also create your API key. You can see in the left navbar there is an option to create an API key.

![Dashboard section to create an API Key](https://cdn.hashnode.com/res/hashnode/image/upload/v1748402705839/4d59bd9c-a4c8-42d0-a218-530f33b92afc.png align="center")

### 5\. Generate a New API Key

Click the **“+ Create new secret key”** button. Give it a name if you want (e.g., "My First App") and OpenAI will show you the key.

**Important:** Copy it now—you won’t be able to see it again!

![OpenAI Dashboard](https://cdn.hashnode.com/res/hashnode/image/upload/v1748398378776/e96e271b-035e-4912-9d1b-5c23b8acb6cc.png align="center")

### 6\. Save Your Key Somewhere Safe

You can store it:

* In a `.env` file (if you’re coding)
    
* In a password manager like Bitwarden or 1Password
    
* In a secure notes app, just not in plain text on your desktop!
    
    Once you've generated an API key, export it as an environment variable in your terminal.
    

### 7\. Set Up Billing

Free credits are available when you sign up, but once those are used, you’ll need to add a card.

Go to Billing overview and add your payment method.

OpenAI uses a **pay-as-you-go** system, so you only pay for what you use.

### 8\. Set Usage Limits (Optional)

Worried about overspending?  
Set monthly limits from Usage Settings so you don’t get surprised by big bills.

### 9\. Follow OpenAI's Guidelines

Make sure your use of the [API](https://keploy.io/blog/technology/why-traditional-api-testing-fails-comparing-shadow-production-replay-techniques) follows OpenAI’s Usage policies.

Avoid using the API for:

* Spam
    
* Misinformation
    
* Hate speech or harassment
    
* Adult or violent content
    

Keep it ethical and useful

---

## Want to automate testing using AI?

If you're looking for a vertical unit testing agent that:

* Runs on your infra **OR**
    
* Uses your API key to test within workflows
    

...then meet **Keploy-gen!** It leverages LLMs to:

1. Understand code semantics
    
2. Generate meaningful unit tests
    

## **Prerequisites**

**AI model Setup** - Set the environment variable **API\_KEY**.

```javascript
export API_KEY=xxxx
```

**API\_KEY** can be from either of one these:

* **OpenAI's GPT-4o** directly **\[preferred\]**.
    
* Alternative LLMs via liteLLM
    
* Azure OpenAI
    
    Then you need to install the Keploy locally use the below command :
    
    ```javascript
     curl --silent -O -L https://keploy.io/install.sh && source install.sh
    ```
    
    Please ensure you've set the API key, as mentioned in pre-requisites above:
    
    ```javascript
    export API_KEY=xxxx
    ```
    

#### **Generating Unit Tests**

* Run the following command in the root of your application.
    
    * **For Single Test File:** If you prefer to test a smaller section of your application or to control costs, consider generating tests for a single source and its corresponding test file:
        
        ```javascript
         keploy gen --sourceFilePath="<path to source file>" --testFilePath="<path to test file for above source file>" --testCommand="npm test" --coverageReportPath="<path to coverage.xml>"
        ```
        

To know more about Keploy-gen: [https://github.com/keploy/keplo](https://github.com/keploy/keploy/blob/main/README-UnitGen.md)[y/blob/](https://github.com/BerriAI/litellm?tab=readme-ov-file#quick-start-proxy---cli)[main/README-UnitGen.md](https://github.com/keploy/keploy/blob/main/README-UnitGen.md)

Keploy-gen is our open-source product! If you're tired of setting up locally, we have an enterprise option where you don’t need to configure anything.

We offer two ways to use it:  
1️⃣ **As a PR Agent**  
2️⃣ **Directly in your VS Code**

You can try all enterprise features **completely free forever**!

**VSCode Extension**: [https://marketplace.visualstudio.com/items?itemName=Keploy.keployio](https://marketplace.visualstudio.com/items?itemName=Keploy.keployio)

**PR Agent**: [https://github.com/marketplace/keploy](https://github.com/marketplace/keploy)

## How Much Does the ChatGPT API Cost?

It depends on which model you use. Here's a rough idea (May 28 2025)

![OpenAI Models pricing](https://cdn.hashnode.com/res/hashnode/image/upload/v1748399394770/0ff6a448-2f07-4e78-a04f-ee13f1bba026.png align="center")

You can always check current pricing here: openai.com/api/pricing

## Conclusion

Whether you're building your first AI-powered app, experimenting with chatbots, or just curious getting your [ChatGPT](https://keploy.io/blog/community/testing-with-chatgpt-epic-wins-and-fails) API key is the first step.

The best part? You don’t need to be an expert to get started. With just a few clicks, you're ready to tap into the power of GPT and build something amazing.

So go ahead, Create your account, grab your key, and let the building begin.

**Hold on before you start building!**

If you're looking for AI-related blogs to inspire you, here are some great reads:

1. **Mastering Mcp To A2a: Everything A Developer Needs To Know,** [**Read here**](https://keploy.io/blog/community/mastering-mcp-to-a2a)
    
2. **Best AI Coding Assistant For Beginners And Experts,** [**Read here**](https://keploy.io/blog/community/best-ai-coding-assistant-for-beginners-and-experts)
    
3. **Best Claude 3.5 Sonnet Style For Code,** [**Read here**](https://keploy.io/blog/community/best-claude-3-5-style-for-code)
    
4. **AI Code Generators,** [**Read here**](https://keploy.io/blog/community/ai-code-generators)
    
5. **Vscode Vs Cursor,** [**Read here**](https://keploy.io/blog/community/vscode-vs-cursor)
    

## FAQs

### 1\. **Is the ChatGPT API free to use?**

You get free credits when you first sign up. After that, it’s pay-as-you-go.

### 2\. **Can I share my API key?**

Please don’t. It’s tied to your account, and if someone else misuses it, you pay the bill.

### 3\. **I lost my key. Can I get it back?**

Nope. OpenAI only shows it once. If you lose it, generate a new one.

### 4\. **Can I delete an old key?**

Yes! Just go to the API key section and click the icon next to the key you want to remove.

### 5\. **What’s the best way to keep it secure?**

Never upload your API key to [GitHub](https://github.com/keploy/keploy) or share it in public. Use environment variables and secret managers (like GitHub Secrets or AWS Secrets Manager).