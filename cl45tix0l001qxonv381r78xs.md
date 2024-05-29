---
title: "Understanding the components of APIs"
seoTitle: "Understanding API basics"
seoDescription: "Even if you're already working with APIs, you might still be confused about the terms like headers, endpoints, status codes, and response body..."
datePublished: Wed Jun 08 2022 16:41:37 GMT+0000 (Coordinated Universal Time)
cuid: cl45tix0l001qxonv381r78xs
slug: understanding-the-components-of-apis
canonical: https://keploy.io/blog/community/understanding-the-components-of-apis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1654699046121/A6NCQrc7v.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1654699709652/_gB6oIRnG.png
tags: api, apis, testing, rest-api, api-basics

---

## Introduction

In this blog, I'll be talking about the technical side of APIs and I'll be telling you what information an API consists of or what components it has.

Even if you're already working with APIs, you might still be confused about the terms like **headers, endpoints, status codes, and response body**. There are more of these terms. So let's understand them one by one.

But before we get started with some technical terms of an API, I would **highly recommend** getting a deep understanding of APIs. To help you out with that, do read the blog about the [**Basics of APIs**](https://medium.com/r/?url=https%3A%2F%2Flittleironical.medium.com%2Fhow-did-i-get-to-know-about-apis-80ec7c055dac) and it won't take you much time to understand, I'm pretty sure about that.

1. ## Endpoint
    

In the simplest terms, an endpoint is **a place from where the data can be accessed**. APIs operate through requests and responses - that is you make a request and the API Endpoint gives a response.

Here's an example, click on this link: [https://api.imgflip.com/get\_memes](https://api.imgflip.com/get_memes)

When you click on the link, you're requesting to get all the meme templates imgflip has in their database, so that API Endpoint is getting response data from the imgflip database and shows it on the website.

Therefore, **an API Endpoint is a URL to access the data**.

Now *you might get confused about the difference between an API and an API Endpoint*, isn't both of them the same as I've shown the same link to demonstrate both?

No, they are not the same. API refers to the whole set of protocols(or you can say code) that allows communication between two systems while an endpoint is a URL that enables the API to gain access to resources on a server.

2. ## Headers
    

Now, this is a little tricky to understand as it's just some extra information for the API calls, or you can say **it contains the metadata that is associated with the API requests and responses**.

> You can consider it as some additional data like timestamp, content type, authorization, etc. that you might not need too often. You will only need this data in case you face issues with the API.

Headers look the same as the formatted API endpoints, that is, in JSON format.

One point to remember here is that these extra data/metadata/additional data are not useless, they are very useful when you will be stuck in any issue.

One of these additional data is sometimes the **authorization that contains your API key**, which is used to access the data or to complete a request successfully. We'll understand this properly.

Let's take an example of you ordering some fast food from your favorite food delivery app. Now, you'll be able to see the delivery guy's exact location. What's happening here is that your app is requesting Google Map Directions to send your delivery guy's real-time location. Here's a catch, Google Map Directions is not a public API that will allow anyone to see anyone's real-time location. So, what it does is that it asks for an **"authentication" key or API Key** as a Header to verify if you're a valid user or not. Then only you're able to access your delivery guy's real-time location.

3. ## HTTP Methods
    

This is the most common part of APIs. **The HTTP Method represents what type of request an API is making.**

There are **7 HTTP Methods** that you need to know:

* **GET** - It's used to get data from a database. The imgflip endpoint that I've provided earlier is an example of a GET request as it only gets the data and shows it to us, we don't have to provide anything from our side. It's the most used HTTP Method.
    
* **POST** - It's used to add new data to a database. The best example of this could be a Sign-up screen on a website. They ask you for your details if you're a first-time user and your new data gets added to their database through a POST request.
    
* **PUT** - It's used to update the existing data in a database. An example of this could be you changing your password on a website. You're sending a PUT request to update your data. It's similar to POST request as it could also be used to add new data to the database.
    
* **PATCH** - It is also used to update existing data just like the PUT request but it is better than that if we talk about updating data. It's because it takes only the data that needs to be updated as input, whereas, PUT needs all the data and that's how it is similar to POST request.
    

Let's see the difference in the requests between PUT and PATCH if one needs to change their password \*PUT Request:

```json
{
  id: 1,
  first_name: "Adam",
  last_name: "jacks",
  age: 21,
  last_password: "123456",
  new_password: "abcdef"
}
PATCH Request:
{
  id: 1, //to determine for whom to change
  last_password: "123456",
  new_password: "abcdef"
}
```

* **DELETE** - This is surely not difficult to guess by yourself. It just deletes the existing data from the database. Note: Not all data at once.
    
* **HEAD** - Similar to the GET request. It gets the Headers(already explained earlier). For example, if a URL might produce a large download, a HEAD request could read its content-length header to check the file size without actually downloading the file. It's like checking what a GET request will return before actually making a GET request.
    
* **OPTIONS** - It's used to check what HTTP Methods an API endpoint supports. It doesn't return any specific data but the HTTP Methods, that is, GET, POST, etc. It's the least used HTTP Method.
    

4. ## Status Codes
    

This is the part where you just have to remember these codes and not understand much. I'll just provide you with some common status codes to remember.

Let's start with understanding what these codes mean in general:

* **1xx - Information responses** : The server is thinking through the request.
    
* **2xx - Success** : The request was successfully completed and the server gave the browser the expected response.
    
* **3xx - Redirection** : You got redirected somewhere else. The request was received, but there's a redirect of some kind.
    
* **4xx - Client Error :**  Page not found. The site or page couldn't be reached.
    
* **5xx - Server Error - Failure**: A valid request was made by the client but the server failed to complete the request.
    

Now, let's see some common status codes that you will face most often during development:

* **200 - OK** :  It means that the request is getting a response.
    
* **201 - Created** : It means that a new item has been created in the database.
    
* **401 - Unauthorized** :  It means the request needs to be authentic. (e.g., if requesting a user's data, you need to provide the user's id)
    
* **404 - Not Found** :  It means a request is valid but there's no data on the server.
    
* **500 - Internal Server Error :** In this case, the error is unknown.
    

5. ## Response Body
    

As the name suggests, it's the body of the response you get out of an API request. A response body consists of Headers, Status Code, and a text body(in JSON format).

**A Response Body is the output of an API request.**

If I talk about an example, whatever you see on [this](https://api.imgflip.com/get_memes) URL is the response body. You will be able to see the Status Code and the Headers if you check this link on [Hoppscotch](https://hoppscotch.io) or [Postman](https://postman.com). These are the API testing tools.

## Conclusion

That's all to it!

As I've mentioned about Hoppscotch and Postman as API testing tools, there's the latest API testing tool that automates the whole process of API testing, the tool is [Keploy](https://github.com/keploy/keploy), you can give it a try if you're planning to test your APIs.

I will be keeping this blog till here. I'll publish more such blogs related to API literacy, so don't forget to give me a follow :)

* by [Hardik Kumar](https://twitter.com/littleironical)