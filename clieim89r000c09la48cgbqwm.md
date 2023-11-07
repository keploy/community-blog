---
title: "The Game of Shadow Testing: The Core of Test Generation"
seoTitle: "The Game of Shadow Testing: The Core of Test Generation"
seoDescription: "The term "Shadow Testing" has become necessary for tech companies nowadays as they have to keep their customers their priority!"
datePublished: Fri Jun 02 2023 12:02:04 GMT+0000 (Coordinated Universal Time)
cuid: clieim89r000c09la48cgbqwm
slug: the-game-of-shadow-testing-the-core-of-test-generation
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1661618056778/-ziR9hnMU.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1661618104798/zNU_N5xhR.png
tags: databases, apis, api-testing

---

## What is Shadow Testing?

The term "Shadow Testing" has become necessary for tech companies nowadays as they have to keep their customers their priority. Rather than testing new features directly on all customers and then making adjustments based on their feedback, companies choose to first test these features on a small group of users. This approach allows them to gather feedback and make necessary adjustments before rolling out the features to a larger audience. This way, they can assess and evaluate the feature's performance and gather feedback before releasing it to a larger audience.

Shadow Testing, also referred to as Dark Launching, is a technique that enables us to test our fully developed software with a small set of users. This testing occurs while the rest of the users continue to use the existing product without the new feature being tested.

![1_jqvZXpPO1kTJUu-fvk0fIQ.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661613210689/lDgsCiuKk.png align="left")

Shadow testing is used to monitor the differences between the current environment and the new environment with a new feature. This is done to reduce any risks before its actual release to the users.

By sampling real traffic from our production environment, without making any changes to our code or affecting the user experience, we gain valuable insights into the behavior of our actual users. This helps us to make informed decisions about the design, development, and optimization of our products.

![1_1b5H2qLczqK0WWanDc_GGA.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661613227384/DZpuqr2I2.png align="left")

Shadow Testing involves creating a replica of the production environment, known as the "shadow environment," to simulate real user traffic. This allows us to test a new feature in a separate environment called the "next environment" (V-Next).

After conducting testing, we compare the responses obtained from both the testing environment and the production environment (V-Current). This step is taken to reduce any potential risks before deploying the new feature to the production environment. In simpler terms, it is a technique that ensures whether the new feature functions correctly before it is officially launched.

By comparing the responses, we verify that the new feature works as intended and does not introduce any negative impact on the existing system. This practice decreases the chances of facing issues when the feature is deployed to the production environment, providing a smoother and safer transition for the new functionality.

## How does API Testing work?

Let's understand this with an example of API testing… There are some tools to implement Shadow Testing like Keploy, Diffy, etc. These tools' primary purpose is to compare V-Current and V-Next responses and then find the differences.

Imagine you own Amazon, and you have an API (a way to access certain information) that fetches product reviews. You recently added a new feature that displays verified or popular users' reviews at the top. After testing the feature, you need to ensure that your APIs are also functioning correctly.

This is where Keploy comes into the picture. It helps test the expected behavior of all the APIs.

Keploy already knows how the APIs should behave based on previous testing. This time, it automatically tests the APIs and compares the differences in their responses. By doing so, it checks if the APIs are still producing the expected output.

In summary, when you add a new feature to Amazon and want to ensure that the APIs associated with it are working as expected, Keploy comes in handy. It tests the APIs and compares their responses to verify that they are still behaving correctly.

## Conclusion

To sum up, Shadow testing does

1. Shadow testing is a technique used to test new features or changes in a real-time production environment without affecting the actual users.
    
2. It involves routing a portion of the production traffic to the new feature or change while keeping the majority of the traffic unaffected.
    
3. Shadow testing allows for the comparison of the behavior and performance of the new feature against the existing system or feature.
    
4. It helps in identifying any potential issues or discrepancies that may arise when the new feature is fully deployed.
    
5. The results of shadow testing provide valuable insights and help in making informed decisions about whether to proceed with the new feature or make further adjustments before full deployment.