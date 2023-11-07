---
title: "APIs vs Webhooks: Make a GitHub Webhook"
seoTitle: "What is a Webhook?, Make a Github Webhook"
seoDescription: "Webhooks allow to notify external services when some event occurs. When the specified event occurs, Webhooks help send POST requests to all the URLs..."
datePublished: Fri Jun 02 2023 12:09:39 GMT+0000 (Coordinated Universal Time)
cuid: clieivyv20hd4yanv9iushxy6
slug: api-vs-webhooks-make-a-github-webhook
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685763358184/91d2da16-087b-4ecb-a726-6c195d78972b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1659261519726/eOc42IeEE.png
tags: github, apis, webhooks, test-automation, keploy

---

> We all like shopping, right? Let me tell you of this one time I really wanted to get myself thigh high black boots and when I checked on Amazon, the boots weren't available in stock. But I was really desperate so I would check Amazon twice or thrice everyday, to the point that it became annoying and it looked like the boots were neither going to be back in stock nor in my cupboard anytime soon. I wanted a way for Amazon to message me as soon as those boots were available.

I got to know that apart from APIs, there are something called as Webhooks. **Webhooks** allow to notify external services when some event occurs. When the specified event occurs, Webhooks help send POST requests to all the URLs given by the user. I wish I had known this earlier and could use this for Amazon to notify me. Definitely, it would've been better to receive a POST request of the boots are available, rather than continuously opening Amazon and checking if they're available or not….

Webhooks are a kind of **reverse API**, it calls you instead of you calling it. You obtain data from an API by "polling" which is the method by which your application periodically sends requests to an API server to check for updated data. On the other hand, a webhook enables the provider to push data to your application as soon as an event takes place. While webhooks allow the server to deliver you info as soon as something happens, APIs must frequently get data from a server.

Now that we know the difference between the two let's dive right into making a webhook for ourselves. First up, we're going to open this [**Github link**](https://github.com/twiliodeved/webhooks-course) and **fork** it, so that we could make our own changes.
![Github repo](https://cdn-images-1.medium.com/max/1200/1*PuGPE1OMPkDcMwf_GoH5LQ.png)

Voila! I now have my own copy. 
![Forked repo](https://cdn-images-1.medium.com/max/1200/1*u4W5YqSLSQ3PcWsVFTwUKQ.png)

After clicking on the **Settings** tab, we can see a **Webhooks** option on the left. "Webhooks allow external services to be notified when certain events happen", that makes a lot of sense now that we know what webhooks are all about. Now let's add a webhook for our repo, click on the **"Add webhook"** button.

![Text](https://cdn-images-1.medium.com/max/1200/1*MzkWxCAEZR0wt3TQhHHzmA.png)

What should our event that triggers a notification on some external service be? We're all familiar with the Github stars, right? Let's keep that as our event, so I want a notification anytime someone stars this repo of mine. 
We can use a mocking tool to capture a webhook. Beeceptor is one such tool which helps us to build a mock API URL in seconds. So now let's head to [**Beeceptor**](https://beeceptor.com/). Now for our mock subdomain name you can type in anything you want, I'm going to type my name "Sejal" and then click on **Create endpoint**.
![Text](https://cdn-images-1.medium.com/max/1200/1*bCuM8mYpv5vRJRmO7c-FFg.png)

So now we have our custom API endpoint as well. Now this route doesn't actually exist, but we've generated a mock just to catch our requests. Just copy this URL.
![Text](https://cdn-images-1.medium.com/max/1200/1*WSAez5cum-lIde5urRTKQg.png)

Paste it in the **Payload URL**, so that's where the information will be sent. We can also just add **"/stars"** in the end to create a new endpoint so that we know this one's checking for stars in our repo.
![x](https://cdn-images-1.medium.com/max/1200/1*v2o6lJ2gy_xxPISJmBiKuQ.png)

We can scroll down and change the **content type** to JSON. We can also choose the events which would trigger our webhook. So since we don't want everything, click on **"Let me select individual events"**, uncheck **"Pushes"** and check **"Stars"**. Lastly, click on **"Add webhook"**.
![x](https://cdn-images-1.medium.com/max/1200/1*-zSanid393jnawZlHABwrQ.png)
![x](https://cdn-images-1.medium.com/max/1200/1*eRD2Aa-CD7ey5D-U53AJmA.png)

Now, if we go ahead and star our repo and head back to Beeceptor, we can see that there was a post to stars.
![x](https://cdn-images-1.medium.com/max/1200/1*i1Y8G8RMGfDOggrO0F4XPg.png)
![x](https://cdn-images-1.medium.com/max/1200/1*npFnZApYD6Znr-PZT28x-g.png)

Now if we click on the first one and click on **view Headers**, when we scroll down we can actually see who the **user** was, their Github **avatar** and a bunch of other information. We can use this to figure out what we want to handle. This was a post request that was sent to the URL and I can figure out what I want to handle.
![x](https://cdn-images-1.medium.com/max/1200/1*62xg5QYBDigUcQRNEF3ZOg.png)

We can actually see this information in a better formatted way in Github itself, if we go back to our webhook and click on **recent deliveries**. Beeceptor was helpful in a way, since not every webhook has this interesting functionality that Github offers, and we will need to catch other webhooks as well in the future.
![x](https://cdn-images-1.medium.com/max/1200/1*fzmj3Seae9sAk7CqmDC9Kw.png)

Now we have a firing webhook and it get's fired when our repo is starred. But let's say this repo is a group project and we want all the people involved in our project to know whenever the repo get's starred. We have a discord server where the people working on the project chat and coordinate for further development of the project. Let's create a new server on discord right now, just to mock the situation. Log into your discord and create a new server, I've named the server "Project Hook". Click on the settings button beside general.
![x](https://cdn-images-1.medium.com/max/1200/1*JMTS4bGtfDr-Kqj3zGLUew.jpeg)


Click on **Integrations** and then **Create webhook**. Change name of the webhook to **"Github Notifier"** and **save changes**. We've built a webhook handler. This is different from what we did with Github earlier. Github made a request to the handler. But this actually handles the request. Both are called webhooks.
![x](https://cdn-images-1.medium.com/max/1200/1*f1-HhOX0rCCZCSb2rNyNcw.png)
 
We create a **new webhook** in Github. Now **copy** the webhook URL. And **paste** it in the **Payload URL** in our new webhook on Github.  At the end also add **"/github"** since it's important for discord for webhook configurations. Change the **content type** to JSON. Let's just try **"Send me everything"** option this time. Click on **"Add webhook"**.
![x](https://cdn-images-1.medium.com/max/1200/1*3GqoJuaAesi5Qdh34sZhbQ.png)

Now the moment has come. Please **Unstar and Star** your repo. Did you hear a beep sound? Look at your discord notification. Go to your discord server and you can see** "A new star was added"** message in the general channel.
![x](https://cdn-images-1.medium.com/max/1200/1*WPU5FVYrv5THFWPsNDNsmQ.jpeg)

***Congratulations***, we just connected two completely different services to send us a notification every time our repo is starred by somebody. I would like to acknowledge that apart from the various websites, PDFs and YouTube videos, [FreeCodeCamp](https://www.youtube.com/watch?v=41NOoEz3Tzc) helped immensely in increasing my understanding. In case of any doubts/clarifications, please feel free to comment below or reach out to me on my [LinkedIn](https://www.linkedin.com/in/sejal-jain17/).