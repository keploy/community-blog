---
title: "How to  mock backend of selenium tests using Keploy"
seoTitle: "Backend Data Mocking Plugin for Selenium Record Replay"
seoDescription: "Backend Data Mocking Plugin called Keploy for Selenium Record Replay test cases"
datePublished: Mon Oct 17 2022 08:49:44 GMT+0000 (Coordinated Universal Time)
cuid: cl9cjcnef000309l2akplgnot
slug: how-to-mock-backend-of-selenium-tests-using-keploy
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1663168083303/jkcrikkd2.png
tags: backend, apis, testing, selenium, frontend-development

---

When I was learning selenium testing, I encountered an issue. Let me explain the issue in detail

Search results for the 1st time


![1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663165932474/H23s0cGAM.png align="left")

Search results for the 2nd time


![2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663165973024/tBDDqTDKV.png align="left")


I searched for ankit on google.com and I got the following result, then after some time I searched the same keyword on google.com, and this time I was getting different suggestions which were leading to the failure of selenium test cases because selenium compares past versus present responses of that particular position

[If you want to learn about selenium testing then you can refer to my blog on How to do selenium testing.](https://hashnode.com/preview/632185485483096daa0aa670)

This issue was making me feel embarrassed to test the front end of many websites, because this phenomenon was common with many dynamic websites, specially e-com websites.

Finally, I came to know about a tool called Keploy. It mocks all the data coming from API calls in its database during seleniums recording, now because of keploy during the selenium replay, data comes from the moc database instead of live database. Now because of a static data source (moc database made by keploy) deflection on responses present over UI decreases.

Now let me show you a tutorial

Prerequisites

1. [golang](https://go.dev/dl/)

2. [Docker](https://docs.docker.com/get-docker/)

3. [GCC compiler](https://sourceforge.net/projects/tdm-gcc/)

4. [selenium IDE extension](https://chrome.google.com/webstore/detail/selenium-ide/mooikfkahbdckldjjndioackbalphokd?hl=en)

5. [keploy](https://github.com/keploy/keploy)

Firstly we need to install keploy in our PC

```
git clone https://github.com/keploy/keploy.git
```
Start your docker

Now come inside this directory and run this project in docker by putting the command mentioned below

```
docker-compose -f docker-compose-dev.yaml up
```
Now we will install Keploy browser extension from the link present below

```
https://github.com/keploy/browser-extension/releases
```
after installing the browser extension go inside your extension manager and switch on the developer mode, after that go on the load unpacked button, which appears just after switching to developer mode, and import your extension file from the expected location.

Now your extension is ready to use along with that your keploy server is also running in docker

Now we will test google.com where selenium will record all the test cases but while replaying data will come from keploy moc database.

Now start testing google.com on your selenium extension.

step-1 Create a new project

![a.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166457115/3W_FmMd7K.png align="left")

![b.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166471249/v_f-KNukE.png align="left")

step-2 Give a title to your 1st test case

![c.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166568666/SpHaKFrk_.png align="left")

![d.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166586215/gE8oBe2J-.png align="left")

![e.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166599945/BlKdlHEP7.png align="left")

step-3 Put the URL of the desired website, In our case, its google.com

![f.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166675667/nw25Ui5Fd.png align="left")

step-4 Press on the record button present in the top right corner

![g.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166733802/2ya9ogCd_.png align="left")

step-5 Now default browser will open, In our case, it's the chrome browser.

step-6 Perform your desired actions and selenium will start recording the positions and actions along with the data present at that place.

step-7 In our case we are doing a google search with the keyword oss. We can see many search results present in the suggestion but we will go with the 1 st option

![h.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166806663/0j-5wtuJW.jpeg align="left")

![i.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166821353/JxYcqjiKO.jpeg align="left")

step-8 We know that our keploy server is already running in the background and keploy extension is present in the chrome browser. In the background, our keploy server is recording all the data coming from APIs on the front end in mongo DB. Let's see inside mongo DB using mongo compass.

![j.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1663166899039/0mVgT_xmA.jpeg align="left")

step-9 Now we will replay the recorded data in selenium, but this time data will not come from live servers of google, instead of that data will come from the moc database made by keploy

![k.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1663167154789/yJEE-t_PV.jpeg align="left")

![l.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1663167167209/yQcMpBeDJ.jpeg align="left")

step 10 Now because of static database all the selenium test cases will pass.

![m.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663167227027/SnAHrzaC-.png align="left")

In this way, keploy solves the drawbacks of selenium and makes frontend testing easier.













