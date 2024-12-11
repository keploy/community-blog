---
title: "Automate GitHub Notifications with Webhooks"
seoTitle: "Automate GitHub Notifications with Webhooks"
seoDescription: "Webhooks allow to notify external services when some event occurs. When the specified event occurs, Webhooks help send POST requests to all the URLs."
datePublished: Fri Jun 02 2023 12:09:39 GMT+0000 (Coordinated Universal Time)
cuid: clieivyv20hd4yanv9iushxy6
slug: api-vs-webhooks-make-a-github-webhook
canonical: https://keploy.io/blog/community/apis-vs-webhooks-make-a-github-webhook
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685763358184/91d2da16-087b-4ecb-a726-6c195d78972b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1659261519726/eOc42IeEE.png
tags: github, apis, webhooks, test-automation, keploy

---

Let me tell you about the time I desperately wanted thigh-high black boots. I checked Amazon daily, sometimes two or three times, only to find they were out of stock. It became frustrating and time-consuming, and I thought: *Wouldn't it be great if Amazon could notify me the moment those boots were back in stock?*

## What are Webhooks?

This brings us to the concept of **webhooks**. Unlike APIs, webhooks allow external services to notify you when a specific event occurs. For example, if Amazon had a webhook, it could send a notification to your application the moment those boots became available. Webhooks achieve this by sending POST requests to a URL you specify, enabling real-time updates without the need for constant manual checks.

## How webhooks are different from API's?

Think of webhooks as a kind of *reverse API*. Instead of your application repeatedly requesting (or "polling") data from a server, webhooks push updates to you as events occur. This fundamental difference makes webhooks ideal for real-time notifications, while APIs are better suited for scenarios requiring on-demand data retrieval.

## Setting Up a Webhook for GitHub Stars

Now that we know the difference between the two let's dive right into making a webhook for ourselves.

1. **Fork a GitHub Repository:** Start by [forking a repository](https://github.com/TwilioDevEd/webhooks-course). This will create a personal copy you can modify.
    

![Github repo](https://cdn-images-1.medium.com/max/1200/1*PuGPE1OMPkDcMwf_GoH5LQ.png align="left")

Voila! I now have my own copy.

![Forked repo](https://cdn-images-1.medium.com/max/1200/1*u4W5YqSLSQ3PcWsVFTwUKQ.png align="left")

2. **Access Webhooks:** Go to the repository's settings and select "Webhooks" from the sidebar. "*Webhooks allow external services to be notified when certain events happen*", that makes a lot of sense now that we know what webhooks are all about. Now let's add a webhook for our repo, click on the **"Add webhook"** button.
    

![Text](https://cdn-images-1.medium.com/max/1200/1*MzkWxCAEZR0wt3TQhHHzmA.png align="left")

3. **Specify the Event:** Choose the "Stars" event as the trigger.  
    We can use a mocking tool to capture a webhook. **Beeceptor** is one such tool which helps us to build a mock API URL in seconds. So now let's head to [**Beeceptor**](https://beeceptor.com/). Now for our mock subdomain name you can type in anything you want, I'm going to type my name "Sejal" and then click on **Create endpoint**.
    

![Text](https://cdn-images-1.medium.com/max/1200/1*bCuM8mYpv5vRJRmO7c-FFg.png align="left")

So now we have our custom API endpoint as well. Now this route doesn't actually exist, but we've generated a mock just to catch our requests. Just copy this URL.

![Text](https://cdn-images-1.medium.com/max/1200/1*WSAez5cum-lIde5urRTKQg.png align="left")

Paste it in the **Payload URL**, so that's where the information will be sent. We can also just add **"/stars"** in the end to create a new endpoint so that we know this one's checking for stars in our repo.

![x](https://cdn-images-1.medium.com/max/1200/1*v2o6lJ2gy_xxPISJmBiKuQ.png align="left")

We can scroll down and change the **content type** to JSON. We can also choose the events which would trigger our webhook. So since we don't want everything, click on **"Let me select individual events"**, uncheck **"Pushes"** and check **"Stars"**. Lastly, click on **"Add webhook"**.

![x](https://cdn-images-1.medium.com/max/1200/1*-zSanid393jnawZlHABwrQ.png align="left")

![x](https://cdn-images-1.medium.com/max/1200/1*eRD2Aa-CD7ey5D-U53AJmA.png align="left")

Now, Let’s star our repo and head back to Beeceptor, we can see that there was a post to stars.

![x](https://cdn-images-1.medium.com/max/1200/1*npFnZApYD6Znr-PZT28x-g.png align="left")

Now if we click on the first one and click on **view Headers**, when we scroll down we can actually see who the **user** was, their Github **avatar** and a bunch of other information. We can use this to figure out what we want to handle. This was a post request that was sent to the URL and I can figure out what I want to handle.

![x](https://cdn-images-1.medium.com/max/1200/1*62xg5QYBDigUcQRNEF3ZOg.png align="left")

We can actually see this information in a better formatted way in Github itself, if we go back to our webhook and click on **recent deliveries**. Beeceptor was helpful in a way, since not every webhook has this interesting functionality that Github offers, and we will need to catch other webhooks as well in the future.

![x](https://cdn-images-1.medium.com/max/1200/1*fzmj3Seae9sAk7CqmDC9Kw.png align="left")

Now we have a firing webhook and it get's fired when our repo is starred.

## Integrating Webhooks with Discord

Suppose you're working on a group project and want to notify your team on Discord whenever your repository gets starred. Here's how you can set it up:

1. **Create a Discord Server:** Log in to Discord and create a server (e.g., "Project Hook").
    

![x](https://cdn-images-1.medium.com/max/1200/1*JMTS4bGtfDr-Kqj3zGLUew.jpeg align="left")

2. **Set Up a Discord Webhook:** Navigate to the server settings, go to "Integrations," and create a new webhook. Name it (e.g., "GitHub Notifier") and copy the webhook URL.
    

![x](https://cdn-images-1.medium.com/max/1200/1*f1-HhOX0rCCZCSb2rNyNcw.png align="left")

3. **Configure GitHub:** Paste the Discord webhook URL into your GitHub webhook settings and add `/github` at the end. Select "Send me everything" for event types.
    

![x](https://cdn-images-1.medium.com/max/1200/1*3GqoJuaAesi5Qdh34sZhbQ.png align="left")

4. **Test the Integration:** Star your repository and check Discord for a notification in the general channel.
    

![x](https://cdn-images-1.medium.com/max/1200/1*WPU5FVYrv5THFWPsNDNsmQ.jpeg align="left")

***Congratulations***, we just connected two completely different services to send us a notification every time our repo is starred by somebody. I would like to acknowledge that apart from the various websites, PDFs and YouTube videos, [FreeCodeCamp](https://www.youtube.com/watch?v=41NOoEz3Tzc) helped immensely in increasing my understanding.

## Enhancing Webhook Security with Secret Tokens

Security is crucial when dealing with webhooks. Here's how you can secure your webhooks:

1. **Generate a Secret Token:** Specify a secret token in your webhook settings on GitHub.
    
2. **Validate the Signature:** In your webhook handler, validate the `X-Hub-Signature` header by comparing the HMAC hash of the payload (using your secret) with the provided signature.
    
3. **Implement Verification:** Ensure this validation step is implemented in your webhook handler to protect against tampered or unauthorized requests.
    

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

GitHub webhooks are a powerful tool for automating notifications and real-time updates. By using webhooks, you can stay informed about repository activity without manually checking for updates. Whether you're tracking stars or integrating with services like Discord, webhooks streamline workflows and enhance collaboration.

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