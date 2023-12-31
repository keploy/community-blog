---
title: "How did I get to know about APIs?"
seoTitle: "How did I get to know about APIs"
seoDescription: "Learn the basics of APIs with the core understanding of why we need them, what does it actually mean, and how we use them in our daily life."
datePublished: Wed Jun 01 2022 15:01:29 GMT+0000 (Coordinated Universal Time)
cuid: cl3vpv6pw00haq2nvbhrm7jik
slug: how-did-i-get-to-know-about-apis
canonical: https://keploy.io/blog/community/how-did-i-get-to-know-about-apis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1654093522303/o82q0jXzw.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1654094357721/qOoJ_ru3j.png
tags: api, apis, beginners, rest-api, api-basics

---

Before I actually integrated an API in my application, the term “API” sounded like a*n un-understandable term* to understand for a noob developer like me.

Today, whenever building a full-fledged application with my team, I always think about what APIs would be needed for the application. I also get annoyed if I don’t get proper documentation of those APIs.

I, a noob developer, was able to understand the actual meaning of that *un-understandable term* after I integrated it with an application.

But I see plenty of my friends who don’t understand what API actually means. They seem confused while explaining it to me.

Hey.. Don’t You Worry!

That’s the reason I’m here for. I’ll explain to you:

1. How do we use these APIs in our daily life
    
2. What does the term API mean
    
3. Why do need API in our lives
    
4. How an actual API looks like
    

...

**1\. How do we use these APIs in our daily life?**

Trust me, we do use APIs in our daily life!

Let’s start with some common APIs that we all use in our everyday. The most prominent example would be:

**a. Login Authentication** on various websites. You must have seen the buttons saying “Login with Google”, “Login with Facebook”, and “Login with Apple” along with the normal Email and Password login fields.

> Login page of Spotify

They are so convenient to use, isn’t it? They don’t ask for your email or password, you just have to click on it, and boom! you’re authenticated. How cool is that? and who remembers their password anyway. I always go for the “forgot password” option if not with Google, Facebook, or Apple authentication.

So, wherever you see these “Login with XYZ Company”, it means that XYZ Company has provided their APIs to authenticate users through their database.

**b. Ecommerce Payments** — If you have ever bought something from any e-commerce website, you’ve definitely seen different options for payment like GPay, PhonePe, and PayTM under the UPI section, right?

> Payment options in Flipkart

So why do you see GPay, PhonePe, and PayTM on all the e-commerce websites while you pay for the product?

It’s because these companies have provided their APIs which the e-commerce companies use to make their payment process simple.

**c. Google Map Directions** — Let’s say you’ve ordered something through your food delivery app like Zomato or Swiggy. Now you will be able to see your delivery person’s location, so, how is this happening?

> Order tracking page of a Food delivery app

So the map that you’re watching on your mobile is provided by Google Maps and your food delivery app is asking for your delivery person’s current location from Google Maps. Hence with the help of APIs, Google Maps is able to send that particular data to your mobile.

...

**2\. What does the term API mean?**

By definition, an API or Application Programming Interface is a connection between two computer programs.

In simpler terms, API is a middleman that carries the data between the application and the database.

> Let’s understand this line using the most popular example of a customer ordering some food in a restaurant.

Imagine you’re sitting at a table in a restaurant with a menu of choices to order from. The kitchen is the part of the “system” that will prepare your order. What is missing is the critical link to communicate your order to the kitchen and deliver your food back to your table. That’s where the waiter or API comes in. The waiter is the messenger — or API — that takes your request or order and tells the kitchen — the system — what to do. Then the waiter delivers the response back to you, in this case, it is the food.

In technical terms, if we talk about a website, API is software that connects the frontend(*the web pages that you see and interact with*) of any website to its database so that the data on the database could be shown on the website. And also the data on the database could be added, modified, or deleted using the frontend of the website.

Still, there is more to it, let’s understand that in the next part.

...

**3\. Why do need API in our lives?**

An API is needed to connect an application to a database.

Here, I used “a database”, which means that APIs can access any database as soon as they have permission to access that database.

> Let’s take an example of Amazon, you see a bunch of products on their home page, they have over 12 million products in their own database.

To show a product’s data on their website:

They have to request the API to get a particular data from the database Then that API requests the database to provide that particular data Once received, API gives back the same data to the frontend that is being received from the database.

**This is how an application connects to its own database.**

**Now talking about connecting with other databases!**

As I’ve mentioned earlier, e-commerce has UPI as one of its payment methods. So what happens here is that the actual payment happens through the company you choose to pay to Amazon. Let’s say you chose PayTM, so you are actually doing a normal UPI payment to Amazon just like you do UPI payment to any other person.

> Now for Amazon to check if the transaction has happened:

It requests PayTM’s API to check if a particular user has paid the required money to Amazon or not Then that API requests the same to PayTM’s database If the database responds with a yes, the API tells Amazon that the user has paid the amount. That’s it!

That’s all the processes that happen behind the scenes.

And now you know how API connects an application with their own database as well as other companies’ databases too.

...

**4\. How an actual API looks like?**

There is a public API provided by [imgflip](https://imgflip.com/) that anyone can use in their personal project, I would like you to [check it out by yourself](https://api.imgflip.com/get_memes).

You see, that is how cluttered it looks in the first place. Let me just show it in a proper JSON format.

> JSON(JavaScript Object Notation) is a standard text-based format for representing structured data.

```plaintext
{
  "success": true,
  "data": {
    "memes": [
      {
        "id": "181913649",
        "name": "Drake Hotline Bling",
        "url": "https://i.imgflip.com/30b1gx.jpg",
        "width": 1200,
        "height": 1200,
        "box_count": 2
      },
      {
        "id": "87743020",
        "name": "Two Buttons",
        "url": "https://i.imgflip.com/1g8my4.jpg",
        "width": 600,
        "height": 908,
        "box_count": 3
      },
      {
        "id": "112126428",
        "name": "Distracted Boyfriend",
        "url": "https://i.imgflip.com/1ur9b0.jpg",
        "width": 1200,
        "height": 800,
        "box_count": 3
      },
      ...(more data)
    ]
  }
}
```

This particular API contains a lot of meme templates that you can display on your own project.

So, this is what all the “API responses” look like whether it’s an API created by Amazon or it’s created by a noob developer like me. It is basically a JSON file hosted online.

And this is what an “API request” looks like:

```plaintext
fetch('https://api.imgflip.com/get_memes')
  .then(response => response.json())
  .then(json => console.log(json))
```

...

Yayyy! You’ve learned it!

Now you can explain the concept of APIs to your friends who are still confused about what exactly is an API and what it does.

If you find this blog insightful and want to know more about the components of API, stay tuned for the next blog!

If you want to get more gist around APIs, check out trending API tools:

* [Keploy.io](https://github.com/keploy/keploy) — a no-code API testing platform.
    
* [Hoppscotch.io](https://hoppscotch.io) — an open-source API development ecosystem.
    
* [Postman.com](https://postman.com) — an API platform for building and using APIs.