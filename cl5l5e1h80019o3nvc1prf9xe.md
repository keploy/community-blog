---
title: "My journey of Automating Test Cases!"
seoTitle: "My journey of Automating Test Cases!"
datePublished: Thu Jul 14 2022 14:50:00 GMT+0000 (Coordinated Universal Time)
cuid: cl5l5e1h80019o3nvc1prf9xe
slug: my-journey-of-automating-test-cases
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1657194537485/B2wOktZEw.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1657194619647/yW6giGBkF.webp
tags: postman, api, apis, testing, test-driven-development

---


Hey! Testing APIs by making API calls and checking if the response is right or not (manually) is a frustrating task.🥱

I have been exploring different ways to automate testing. So, next time I don’t have to execute the same things again.🕵️‍♀️

Over the period of last few weeks, I have been exploring and experimenting with various testing tools and frameworks, and during that. I explored writing test-automation scripts but I could see forehand that it was going to take time. So, I looked into all these popular fancy **Test Automation Tools** and tried 2 from the extreme ends... **Postman** (popular, mature closed-source, widely adopted) and **Keploy** (no-code, new open-source, secondarily adopted). 

I found that with Postman, along with API management I can write test scripts for my cases and assert the API responses using the Postman **test snippet** feature.

I initially used the Postman test snippet to write test cases and used it to run the test collection that I built eventually but I was a little exhausted because the **test cases broke so often**. 

Why did they break? It was not me! It was not the code changes! **It was the database!!** Many times the API response changed because my team member changed the database my application was talking to! ...😢

I soon realised I need to change all the test scripts I wrote in Postman and make sure NOBODY messes up with my test database or else I'd need to do the same again.

Then, I looked for more testing automation tools like Rest Assured, Selenium, Katalon, etc… but these also required writing test cases, and test database. 

After this, I started looking for no-code test automation tools and found Keploy with which I was able to record Postman API calls as a test case and the best part was I didn't need to keep my test database.

In this article, I will brief about the steps that I took to automate my test cases on both Postman and Keploy:

1. Setting up a sample application(URL Shortener) locally — optional

2. Creating test snippets in Postman

3. Recording test cases in Keploy

Moving on to the first step…

![1_iFZEf3i8zVc54W3Dmr4Fcw.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656686338527/Qzaw1BU7y.png align="center")

Let's take one sample application and write test cases for the same using both Postman and Keploy. Clone and run a **sample URL Shortener** application locally

```
git clone https://github.com/keploy/samples-go && cd samples-go/echo-sql
    
go mod download
``` 

**Start the PostgreSQL** instance in Docker for this sample application.
In Postgres DB the sample application is storing the shortened URL for a given long URL.
```
docker-compose up -d
```
Now open a new terminal and run the sample application
```
go run handler.go main.go
```
Doing this you will start the localhost server for the URL shortener application.

### Now we can try making some API calls

Let’s Post a long URL, say, github.com , which will add a shortened URL in the database.

You can import the following curl into Postman
```
curl --request POST \
  --url http://localhost:8080/url \
  --header 'content-type: application/json' \
  --data '{
  "url": "https://github.com"
}'
```

![Adding an URL for the API call](https://cdn.hashnode.com/res/hashnode/image/upload/v1656695533749/-f8smtQ6N.png align="center")


After making the API call, you will get the shortened URL for github.com

![1_XBRU8kAMJN2fTYP7eqtZ6A.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656696111606/YrJDghNza.png align="left")

Let make another API call to GET the shortened URL content.

![1_jEX2Sru0geODw28Y5PLq5Q.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656696383680/D6bliJyt7.png align="left")

 ```
curl --request GET \
  --url http://localhost:8080/GuwHC
```

![1_oG5T4Jr3tlkdLqRlnx-osQ.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656696419111/egqOZCx_p.png align="left")
Since we understood both the API of the sample application, lets automate the testing for these APIs.

### Creating test snippets in Postman

Moving towards the testing of this API!

I will be making the following testcases here:



1. Status code
2. Response Time

![1_fsKg7sF6C_3WZyH81QADEw.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656707598315/wXT3lQ4I5.png align="center")

Now, as I have done above.

Just to check if my request gives me a status 200/201/202 etc, or to check the response Time, I have to manually write testcases in Postman.

This seems plausible when the testing is done on a small scale.

**What would you do if you have an API with 1000+ data in it and you have to verify each of its content?**

### Recording test cases in Keploy

To get started with Keploy, I will follow the below steps:

```
git clone https://github.com/keploy/keploy.git && cd keploy
docker-compose up
```
Once the Keploy server is up and running, we will start our application server over localhost.

To do that, run the following command in the terminal.

```
docker-compose up -d
go run handler.go main.go
```

Once the server is setup, run your Postman as usual and make API calls.

Keploy locahost can be accessed on localhost:8081 and you can use the following link to get to the test cases directly

```
http://localhost:8081/testlist
```

![1_Ct6UYDGrEyUA29RfHdPwBA.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656708508628/wWoLXvjwA.png align="left")

As you can see, we have our testcases generated by Keploy and now we just need to start testing.

Moving forwards, kill your localhost and run the below command to do run testing

```
go test -coverpkg=./... -covermode=atomic  ./...
```
this should show you have 74.4% coverage without writing any code!

Start your server again with
```
go run handler.go main.go
```
go back to keploy localhost and check the Test runs



![1_mhMufdbp5GCeuoKRQZZUyg.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656708579755/M9Ws-9ULh.png align="left")
TestRun for the API


![1_QbTKnXAKzpY2uLJsBGwfAw.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656708587596/2jSF9D3Ut.png align="left")
TestRuns of our API calls

You can find that Keploy has done in total 6 tests and each tests have Passed.

The major point here is that we didn’t made any test case on our own🤩!


## Concluding

We just did API Testing on both Postman and Keploy and as we experienced,

I had to add test cases using snippets when testing using Postman and the test database needs to be maintained in long term.

Whereas I didn’t have to write a test script to do the testing in Keploy, it's not a very big plus, what I liked the most was that the database calls were also recorded and I don't have to set up that test database again.

This is it for now!... I will be coming up with more approaches I explored with different testing tools. 

Wishing you error-free API delivery! 😁


Don’t forget to give me a follow as I’ll publish more such blogs related to APIs and Testing 😁

— by [Nishant Mishra](https://twitter.com/curlyParadox)



