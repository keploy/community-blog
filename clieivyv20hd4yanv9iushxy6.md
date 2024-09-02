---
title: "Mastering GitHub Webhooks: Automate Notifications & Stay Informed"
seoTitle: "Mastering GitHub Webhooks: Automate Notifications & Stay Informed"
seoDescription: "Webhooks allow to notify external services when some event occurs. When the specified event occurs, Webhooks help send POST requests to all the URLs."
datePublished: Fri Jun 02 2023 12:09:39 GMT+0000 (Coordinated Universal Time)
cuid: clieivyv20hd4yanv9iushxy6
slug: api-vs-webhooks-make-a-github-webhook
canonical: https://keploy.io/blog/community/apis-vs-webhooks-make-a-github-webhook
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685763358184/91d2da16-087b-4ecb-a726-6c195d78972b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1659261519726/eOc42IeEE.png
tags: github, apis, webhooks, test-automation, keploy

---

We all like shopping, right? Let me tell you of this one time I really wanted to get myself thigh high black boots and when I checked on Amazon, the boots weren't available in stock. But I was really desperate so I would check Amazon twice or thrice everyday, to the point that it became annoying and it looked like the boots were neither going to be back in stock nor in my cupboard anytime soon. I wanted a way for Amazon to message me as soon as those boots were available.

## What are Webhooks?

I got to know that apart from APIs, there are something called as Webhooks. **Webhooks** allow to notify external services when some event occurs. When the specified event occurs, Webhooks help send POST requests to all the URLs given by the user. I wish I had known this earlier and could use this for Amazon to notify me. Definitely, it would've been better to receive a POST request of the boots are available, rather than continuously opening Amazon and checking if they're available or not..

## How webhooks are different from API's?

Webhooks are a kind of **reverse API**, it calls you instead of you calling it. You obtain data from an API by "polling" which is the method by which your application periodically sends requests to an API server to check for updated data. On the other hand, a webhook enables the provider to push data to your application as soon as an event takes place. While webhooks allow the server to deliver you info as soon as something happens, APIs must frequently get data from a server.

Now that we know the difference between the two let's dive right into making a webhook for ourselves. First up, we're going to open this [**Github link**](https://github.com/twiliodeved/webhooks-course) and **fork** it, so that we could make our own changes.

![Github repo](https://cdn-images-1.medium.com/max/1200/1*PuGPE1OMPkDcMwf_GoH5LQ.png align="left")

Voila! I now have my own copy.

![Forked repo](https://cdn-images-1.medium.com/max/1200/1*u4W5YqSLSQ3PcWsVFTwUKQ.png align="left")

After clicking on the **Settings** tab, we can see a **Webhooks** option on the left. "Webhooks allow external services to be notified when certain events happen", that makes a lot of sense now that we know what webhooks are all about. Now let's add a webhook for our repo, click on the **"Add webhook"** button.

![Text](https://cdn-images-1.medium.com/max/1200/1*MzkWxCAEZR0wt3TQhHHzmA.png align="left")

What should our event that triggers a notification on some external service be? We're all familiar with the Github stars, right? Let's keep that as our event, so I want a notification anytime someone stars this repo of mine. 

We can use a mocking tool to capture a webhook. **Beeceptor** is one such tool which helps us to build a mock API URL in seconds. So now let's head to [**Beeceptor**](https://beeceptor.com/). Now for our mock subdomain name you can type in anything you want, I'm going to type my name "Sejal" and then click on **Create endpoint**.

![Text](https://cdn-images-1.medium.com/max/1200/1*bCuM8mYpv5vRJRmO7c-FFg.png align="left")

So now we have our custom API endpoint as well. Now this route doesn't actually exist, but we've generated a mock just to catch our requests. Just copy this URL.

![Text](https://cdn-images-1.medium.com/max/1200/1*WSAez5cum-lIde5urRTKQg.png align="left")

Paste it in the **Payload URL**, so that's where the information will be sent. We can also just add **"/stars"** in the end to create a new endpoint so that we know this one's checking for stars in our repo.

![x](https://cdn-images-1.medium.com/max/1200/1*v2o6lJ2gy_xxPISJmBiKuQ.png align="left")

We can scroll down and change the **content type** to JSON. We can also choose the events which would trigger our webhook. So since we don't want everything, click on **"Let me select individual events"**, uncheck **"Pushes"** and check **"Stars"**. Lastly, click on **"Add webhook"**.

![x](https://cdn-images-1.medium.com/max/1200/1*-zSanid393jnawZlHABwrQ.png align="left")

![x](https://cdn-images-1.medium.com/max/1200/1*eRD2Aa-CD7ey5D-U53AJmA.png align="left")

Now, if we go ahead and star our repo and head back to Beeceptor, we can see that there was a post to stars.

![x](https://cdn-images-1.medium.com/max/1200/1*i1Y8G8RMGfDOggrO0F4XPg.png align="left")

![x](https://cdn-images-1.medium.com/max/1200/1*npFnZApYD6Znr-PZT28x-g.png align="left")

Now if we click on the first one and click on **view Headers**, when we scroll down we can actually see who the **user** was, their Github **avatar** and a bunch of other information. We can use this to figure out what we want to handle. This was a post request that was sent to the URL and I can figure out what I want to handle.

![x](https://cdn-images-1.medium.com/max/1200/1*62xg5QYBDigUcQRNEF3ZOg.png align="left")

We can actually see this information in a better formatted way in Github itself, if we go back to our webhook and click on **recent deliveries**. Beeceptor was helpful in a way, since not every webhook has this interesting functionality that Github offers, and we will need to catch other webhooks as well in the future.

![x](https://cdn-images-1.medium.com/max/1200/1*fzmj3Seae9sAk7CqmDC9Kw.png align="left")

Now we have a firing webhook and it get's fired when our repo is starred. But let's say this repo is a group project and we want all the people involved in our project to know whenever the repo get's starred. We have a discord server where the people working on the project chat and coordinate for further development of the project. Let's create a new server on discord right now, just to mock the situation. Log into your discord and create a new server, I've named the server "Project Hook". Click on the settings button beside general.

![x](https://cdn-images-1.medium.com/max/1200/1*JMTS4bGtfDr-Kqj3zGLUew.jpeg align="left")

Click on **Integrations** and then **Create webhook**. Change name of the webhook to **"Github Notifier"** and **save changes**. We've built a webhook handler. This is different from what we did with Github earlier. Github made a request to the handler. But this actually handles the request. Both are called webhooks.

![x](https://cdn-images-1.medium.com/max/1200/1*f1-HhOX0rCCZCSb2rNyNcw.png align="left")

We create a **new webhook** in Github. Now **copy** the webhook URL. And **paste** it in the **Payload URL** in our new webhook on Github. At the end also add **"/github"** since it's important for discord for webhook configurations. Change the **content type** to JSON. Let's just try **"Send me everything"** option this time. Click on **"Add webhook"**.

![x](https://cdn-images-1.medium.com/max/1200/1*3GqoJuaAesi5Qdh34sZhbQ.png align="left")

Now the moment has come. Please **Unstar and Star** your repo. Did you hear a beep sound? Look at your discord notification. Go to your discord server and you can see\*\* "A new star was added"\*\* message in the general channel.

![x](https://cdn-images-1.medium.com/max/1200/1*WPU5FVYrv5THFWPsNDNsmQ.jpeg align="left")

***Congratulations***, we just connected two completely different services to send us a notification every time our repo is starred by somebody. I would like to acknowledge that apart from the various websites, PDFs and YouTube videos, [FreeCodeCamp](https://www.youtube.com/watch?v=41NOoEz3Tzc) helped immensely in increasing my understanding.

## Enhancing Webhook Security with Secret Tokens

When setting up webhooks, it's crucial to consider security. One effective way to secure your webhooks is by using secret tokens. A secret token is a string that you generate and include in the webhook configuration. This token is sent along with each POST request in the `X-Hub-Signature` header, which GitHub calculates using the HMAC hex digest of the payload.

Here's how you can secure your webhook with a secret token:

1. **Generate a Secret Token**: When creating a webhook, GitHub allows you to specify a secret. This secret will be used to generate the HMAC hash of the payload.
    
2. **Validate the Signature**: In your webhook handler, you should validate the `X-Hub-Signature` header. Compare the HMAC hash of the payload (using your secret) with the signature provided in the header. If they match, the request is authentic. This ensures that the payload has not been tampered with and that it indeed comes from GitHub.
    
3. **Implementing in Your Code**: Make sure to implement this check in your webhook handler to prevent unauthorized or malicious requests from being processed.
    

## Troubleshooting Common Webhook Issues

While setting up and using webhooks, you might encounter a few common issues. Here’s how to troubleshoot them:

1. **Failed Deliveries**:
    
    * **Check the Payload URL**: Ensure the URL is correct and accessible. If your server is down or the URL is incorrect, the delivery will fail.
        
    * **Inspect the Response**: GitHub provides details about webhook deliveries. If a delivery fails, check the **Recent Deliveries** section to see the HTTP response code and error message.
        
    * **Network Issues**: Ensure there are no firewall or network issues preventing the webhook from reaching your server.
        
2. **Incorrect Configurations**:
    
    * **Event Selection**: Double-check that you've selected the correct events to trigger your webhook. For example, if you want notifications for stars, ensure only the **Stars** event is checked.
        
    * **Content Type**: Make sure the content type (e.g., JSON) matches what your server expects. Mismatched content types can lead to parsing errors.
        
3. **Signature Verification Failures**:
    
    * **Secret Token Mismatch**: If you're using a secret token, ensure the token in your webhook configuration matches what your server uses to verify the signature.
        
    * **Incorrect Signature Header**: Verify that your code correctly extracts and processes the `X-Hub-Signature` header.
        

## Conclusion

GitHub Webhooks offer a powerful way to stay informed about activity on your repositories without constantly checking for updates manually. By setting up webhooks, you can receive notifications in real-time whenever specific events, like stars or pushes, occur. Whether you're a developer tracking changes to your codebase or a project manager keeping your team in the loop, GitHub webhooks streamline the process and make collaboration more efficient.

So, why not take advantage of this handy feature and let webhooks do the monitoring for you? With just a few simple steps, you can stay connected and informed, allowing you to focus on what matters most.

## Frequently Asked Questions

### **What is a GitHub webhook, and how does it differ from polling via API?**

A GitHub webhook is a mechanism that allows GitHub to notify external services when certain events, like repository stars or pushes, occur. Unlike polling, where your application periodically checks for updates, webhooks enable GitHub to push data to your application as soon as an event happens.

### **How do I set up a GitHub webhook for my repository?**

Setting up a GitHub webhook involves a few simple steps. First, navigate to your repository's settings on GitHub, then find the "Webhooks" section. From there, you can add a new webhook by specifying the events you want to trigger it, providing a URL where GitHub should send notifications, and configuring other settings as needed.

### **Can I use a tool like Beeceptor to test my GitHub webhook?**

Yes, tools like Beeceptor can be very useful for testing GitHub webhooks. Beeceptor allows you to create a custom API endpoint where GitHub can send webhook payloads. This lets you simulate webhook events and ensure that your application is handling them correctly.

### **What events can trigger a GitHub webhook?**

GitHub webhooks support a variety of events, including pushes, pulls, stars, forks, and more. You can choose which events you want to trigger your webhook when setting it up in your repository's settings. This allows you to customize the notifications you receive based on your specific needs.

### **Can I use GitHub webhooks to integrate with other services, like Discord?**

Absolutely! GitHub webhooks can be integrated with a wide range of external services, including Discord, Slack, and email providers. By configuring webhooks to send notifications to these services, you can keep your team informed about repository activity in real-time, streamlining communication and collaboration.