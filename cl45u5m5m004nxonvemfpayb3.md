---
title: "Understanding HTTP and HTTPS as a Beginner"
seoTitle: "HTTP and HTTPS as a Beginner"
seoDescription: "A beginner's guide to understanding HTTP and HTTPS, their features, methods, and SSL/TLS encryption for secure web communication"
datePublished: Wed Jun 08 2022 16:59:16 GMT+0000 (Coordinated Universal Time)
cuid: cl45u5m5m004nxonvemfpayb3
slug: understanding-http-and-https-as-a-beginner
canonical: https://keploy.io/blog/community/understanding-http-and-https-as-a-beginner
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1654618370657/F6BhWsD91.png
tags: ssl, http, https, tls, rest-api

---

## Introduction

When I started with web development, I was dreadfully curious about knowing how a website gets delivered from server to web browser. I came to know, that it happens through various network layers. Then I explored HTTP which you generally see before the name of any website in your chrome browser URL bar. So basically HTTP is an application layer protocol that helps web-based applications, communicate with web browsers or we can say that HTTP is a messenger of the web.

![photo_2022-06-06_23-32-40.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1654618550184/hqZXvTvIx.jpg align="left")

HTTP was designed for fetching HTML pages but along with the time it continuously evolved and with lots of improvement, now a days it is used for transferring a variety of data like images, videos, audio, and documents. etc

## Features of HTTP

Now let’s discuss the two most important features of HTTP:

1. **HTTP is connectionless:** After making the request, the client disconnect from the server, then when the response is ready the server re-establish the connection again and delivers the response
    
2. **HTTP is stateless:** The client and server know about each other just during the current request, if it closes, and the two computers want to connect again, they need to provide information to each other, and the connection is handled as the very first one.
    

## HTTP Methods

HTTP handles two types of methods request and response!!

1. ### Request
    

![photo_2022-06-06_23-32-45.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1654618800422/fLnzJ6tUk.jpg align="left")

**Method** tells us about what to do!

**URL refers to,**

* Uniform resource identifier
    
* It is a set of readable characters
    
* and a way to locate the resource
    

**Header-** Specifies some information and the rules for example the server

2. ### Response
    

![photo_2022-06-06_23-32-49.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1654618951965/rIW-JyhBA.jpg align="left")

It tells the client if the request succeeded or failed. For example, 200 ok, 404 not found

## SSL/TLS Encryption

Now let’s understand what is the meaning of S in HTTPS, So very simply S represents secure, which means that the content travelling between the server to the browser is encrypted so that no one can penetrate or steal the data travelling between the server and the browser

This security is much needed while online payment and logging into some websites, Now we will dig a bit deeper into this security protocol or you can say that now we will understand how things work in this encryption.

So firstly this encryption is achieved by SSL/TLS certificate provided by a third-party CA (certification authority) like Godaddy, cloud flare, sectigo .etc to the websites and an SSL certificate stays inside our browser which is used for matching and verifying the approved certificate which our website is carrying. ([you can check out your browser SSL by clicking](https://clienttest.ssllabs.com:8443/ssltest/viewMyClient.html))

Now let’s understand SSL/TLS The complete form of SSL is a secure socket layer and TLS is Transport layer security which is of two types:

1. ### Asymmetric
    

![photo_2022-06-06_23-32-54.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1654624532709/CAst5v4Y4.jpg align="left")

In Asymmetric encryption, encryption happens with the browser’s public key but decryption happens with the server’s private key. This kind of encryption is achieved by two types of keys that’s why it is known as an asymmetric method. ([for understanding, encryption key exchange refer to this video](https://youtu.be/T4Df5_cojAs))

2. ### Symmetric
    

![photo_2022-06-06_23-32-58.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1654626073435/7ztlJxI9N.jpg align="left")

In symmetric encryption, both encryption & decryption occurs with the same key, that’s why this method is known as a symmetric method. [for understanding, encryption key exchange refer to this video](https://youtu.be/T4Df5_cojAs)

While SSL encryption, both symmetric and asymmetric method works combine to give more security!!

## Conclusion

For understanding a bit deeper and regarding any confusion, feel free to ask in the comment box, I will be happy to help you regarding this topic. I generally reply within 24hr!

After Exploring a Lot of things about HTTP and HTTPS, I explored APIs which also fascinated me to deep dive into it and write a blog about it, So stay tuned for it, But if you are aware of APIs you must, Try out this no-code API testing tool, It reduces your effort while API testing. ([GitHub Link](https://github.com/keploy/keploy))