---
title: "Create API Rate Limiting with Token Bucket 🪣"
seoTitle: "Building API Rate Limit with Token Bucket"
seoDescription: "Learn about the Token Bucket algorithm and its implementation for API rate limiting in Node.js. This tutorial provides a step-by-step guide with code."
datePublished: Thu May 16 2024 06:30:06 GMT+0000 (Coordinated Universal Time)
cuid: clw8vfm4q000s09kz588t5ttr
slug: create-api-rate-limiting-with-token-bucket
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1715346407766/c7a35c96-f30e-4ed6-99c7-351253de18cc.jpeg
tags: api, rate-limiting

---

In this article, We will try to explore **Token Bucket** algorithm and its implementation in NodeJS for ***API Rate Limiting*** in very simple terms.

### What is Token Bucket ?

**Token Bucket** is an algorithm which is used to limit our resources or server usage. It is an algorithm in which we have some finite amount of tokens on our server and whenever a request is made, a token is used for that request full fulfillment. When there are requests more than the number of tokens, then the server denies or awaits all the requests until the tokens are refilled.

The tokens can be refilled in various ways. Two of them are as follows :

* whenever a request is fulfilled, the token used by the user is returned back to the server.
    
* refilling the tokens after a specific interval of time.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715346573626/fdecfaa9-210f-407a-a35c-63de7fa7fffc.png align="center")

### Implementation of Token Bucket

1. Setup a new npm project and set the "type":"module".
    
    ```bash
     npm init -y
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1713298721958/dd6aec84-6a75-4ac8-a434-6bd92b9696c2.png?auto=compress,format&format=webp align="left")
    
2. Install express.
    
    ```bash
     npm i express
    ```
    
3. Create a new file, name it anything, let's say "index.js". Write a basic express server code in it.
    
    ```javascript
     import express from 'express';
     const app = express();
    
     app.get('/',(req,res) => {
         res.send('Hello There !!');
     })
    
     const PORT = process.env.PORT || 8000;
     app.listen(PORT,() => {
         console.log(`Server is running on PORT : ${PORT}`);
     })
    ```
    
    Now we'll be rate limiting the request to '/'. We'll be programming a middleware to achieve this.
    
4. Create a new file for the middleware, let's say, "rateLimiter.js" and write the syntax of the middleware function and export it.
    
    ```javascript
     export const rateLimiter = (req,res,next) => {
    
     }
    ```
    
    ```javascript
     const tokens = ["AAAA","AAAA","AAAA","AAAA","AAAA"];
     var time = new Date().getTime();
    
     export const rateLimiter = (req,res,next) => {
    
     }
    ```
    
    In this code, ***tokens*** is the array of the tokens which the user can use to make a request to the server. ***time*** is a variable in which we store the current time in milliseconds.
    
    ```javascript
     const tokens = ["AAAA","AAAA","AAAA","AAAA","AAAA"];
     var time = new Date().getTime();
    
     export const rateLimiter = (req,res,next) => {
        if(tokens.length){
             tokens.pop();
             next();
         }
         else{
             res.send(`Service not available due to excess requests !! , ${new Date().getTime()-time}`);
         }
     }
    ```
    
    We have added simple if else condition in this, if any request comes and if the ***tokens*** array has a token, then a token will pop out and the request will be allowed, else the request will be denied.
    
    After the tokens array becomes empty, we need to refill it.
    
    ```javascript
     const tokens = ["AAAA","AAAA","AAAA","AAAA","AAAA"];
     var time = new Date().getTime();
    
     export const rateLimiter = (req,res,next) => {
        if(tokens.length){
             tokens.pop();
             next();
         }
         else{
             if(new Date().getTime()-time >= 20000){
                 const len = tokens.length;
                 for(let i=0;i<5-len;i++){
                     tokens.push("AAAA");
                 }
                 time = new Date().getTime();
                 rateLimiter(res,res,next);
             }
             else{
                 res.send(`Service not available due to excess requests !! , ${new Date().getTime()-time}`);
             }
         }
     }
    ```
    
    In this code, we changed a little bit of else condition. We are calculating the time difference with "**new Date().getTime()-time**". If this difference exceeds 20000 milliseconds i.e. 20 sec, the tokens array will be refilled to 5 tokens.
    
5. But this code has a problem. In this code, whenever the server is started, the time variable gets initialized. Suppose after 20 seconds the first request is made. Then, by this code we will be able to make 10 requests in a span of 20 seconds because after 20 seconds when all the 5 tokens are used, the if condition of the outer else condition will run and it will refill the array with 5 more tokens immediately. But by our code, we only want 5 requests per 20 seconds.
    
6. With a smaller change in the present code, we can achieve the desired result.
    
    ```javascript
     const tokens = ["AAAA","AAAA","AAAA","AAAA","AAAA"];
     var time = new Date().getTime();
    
     export const rateLimiter = (req,res,next) => {
         if(tokens.length && new Date().getTime()-time < 20000){
             tokens.pop();
             next();
         }
         else{
             if(new Date().getTime()-time >= 20000){
                 const len = tokens.length;
                 for(let i=0;i<5-len;i++){
                     tokens.push("AAAA");
                 }
                 time = new Date().getTime();
                 rateLimiter(res,res,next);
             }
             else{
                 res.send(`Service not available due to excess requests !! , ${new Date().getTime()-time}`);
             }
         }
     }
    ```
    
    We changed the outer **if** condition. The conditions are like firstly the server should be having a token and the time should be less than 20 sec, if the time difference is exceeding 20 seconds, the array will be refilled if there are less than 5 tokens in it.
    
7. Now add the middleware to the route.
    
    ```javascript
    import express from 'express';
    import { rateLimiter } from './rateLimiter.js';
    const app = express();
    
    app.get('/', rateLimiter, (req,res) => {
        res.send('Hello There !!');
    })
    
    const PORT = process.env.PORT || 8000;
    app.listen(PORT,() => {
        console.log(`Server is running on PORT : ${PORT}`);
    })
    ```
    

### Congratulations ! You have successfully added Rate Limitation to your server

The final code looks like this.

```javascript
//"index.js"

import express from 'express';
import { rateLimiter } from './middleware/rateLimiter.js';
const app = express();

app.get('/', rateLimiter, (req,res) => {
    res.send("Hello There !!");
})

const PORT = process.env.PORT || 8000;
app.listen(PORT,() => {
    console.log(`Server is running on PORT : ${PORT}`);
})
```

```javascript
// "rateLimiter.js"

const tokens = ["AAAA","AAAA","AAAA","AAAA","AAAA"];
var time = new Date().getTime();

export const rateLimiter = (req,res,next) => {
    if(tokens.length && new Date().getTime()-time < 20000){
        tokens.pop();
        next();
    }
    else{
        if(new Date().getTime()-time >= 20000){
            const len = tokens.length;
            for(let i=0;i<5-len;i++){
                tokens.push("AAAA");
            }
            time = new Date().getTime();
            rateLimiter(res,res,next);
        }
        else{
            res.send(`Service not available due to excess requests !! , ${new Date().getTime()-time}`);
        }
    }
}
```

## Conclusion

Now, I think we have understood how Token Bucket is implemented and will be able to customize the number of requests and tokens in it. If you have any queries, write in the comments and I will try to help you out.

## FAQs:

### What is the Token Bucket algorithm?

The Token Bucket algorithm is used for rate limiting to control the consumption of resources or server usage. It involves maintaining a bucket of tokens, where each token represents permission to perform a specific action or request.

### How does the Token Bucket algorithm work?

Requests consume tokens from the bucket. When the bucket is empty, further requests are either denied or queued until tokens are refilled. Tokens can be refilled at a fixed rate or based on certain conditions.

### What is the purpose of rate limiting in APIs?

Rate limiting helps prevent abuse or overload of APIs by restricting the number of requests a client can make within a specified timeframe. It ensures fair usage of resources and protects the server from being overwhelmed.