---
title: "Canary Testing: A Comprehensive Guide for Developers"
seoTitle: "Canary Testing: A Comprehensive Guide for Developers"
seoDescription: "Imagine you're a miner with a canary in a cage. If the air is toxic, the canary reacts first, giving you a heads-up."
datePublished: Tue Feb 20 2024 05:53:08 GMT+0000 (Coordinated Universal Time)
cuid: clsty8t3l000609jl9whw9mjv
slug: canary-testing-a-comprehensive-guide-for-developers
canonical: https://keploy.io/blog/community/canary-testing-a-comprehensive-guide-for-developers
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708344967449/0a9c7fc9-b16b-4aa6-8544-9521e70995bd.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1708345031804/be2d3675-cb6d-4517-8a4c-fc84d320cde9.png

---

## What's Canary Testing, Anyway?

Imagine you're a miner with a canary in a cage. If the air is toxic, the canary reacts first, giving you a heads-up. Canary testing works in a similar fashion for your software. Instead of releasing the whole flock (users) into a potentially toxic or buggy environment, you release just one canary (a small subset of users) to test the waters.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710236730914/ae59d872-5de9-491d-b482-c74c8b5c2fa1.png align="center")

Once you have a new release ready, you can deploy it to one of the environments. Then, you can direct a small portion of your users (around 5% is recommended) to this canary release. These users will experience the new features, while the other group will not encounter any changes.

## Why Canary?

In the coal mining days, miners used canaries to detect dangerous gases. If the canary stopped singing, it was a sign that something was wrong. In our tech world, our "canaries" are the early adopters who help us sniff out any issues before a full-scale release.

Developers create automated tests for their software's new features and modifications. The changes are deployed to a testing environment where others can explore and interact with the new features. If everything goes smoothly, the new software update is rolled out to the production environment, allowing end users to benefit from the newly added feature.

However, given the nature of software, bugs tend to move into production. As humans, it is impossible to anticipate every potential edge case. Moreover, deadlines and budget constraints add to the pressure.

## Setting Up Your Canary Test

To execute the canary test, two approaches are mainly implemented to achieve reliable outcomes. We'll be using `GoLang` for our examples because, well, Go is awesome. Here are those two approaches:

### Feature Flags

Start by incorporating feature flags into your code. These nifty toggles allow you to enable or disable certain features at runtime. Perfect for controlling who gets to see your new changes.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1710242766881/84965dcb-a9ec-453b-a473-9ac3d8e20821.png align="center")

With the feature flag, you can limit the release to 5% of the users and monitor the key metrics. This approach is handy for business stakeholders who need to test new features before implementing them for everyone. However, while performing a canary test, if any issue is detected during the deployment method, you can easily disable the new features by turning the feature off.

```go
package main

import (
    "fmt"
    "feature" // Assuming a feature package for checking feature status
)

func main() {
    if feature.IsFeatureEnabled("new-feature") {
        // User Input and Processing
        var userInput string
        fmt.Println("Enter your name:")
        _, err := fmt.Scanln(&userInput) // Read user input
        if err != nil {
            fmt.Println("Error reading input:", err)
        } else {
            fmt.Println("Hello,", userInput + "!") // Process input
        }
    } else {
        // Code for the old behavior
        fmt.Println("Hello, User!")
    }
}
```

In this code, the `IsFeatureEnabled` function is expected to perform the actual check for the feature's status. Depending on the result, the program executes either the code intended for the new feature or the code representing the old behavior.

Time to deploy! Let's push our changes but only to a small fraction of our users. In Go, we can use something like this:

```go
// deployment.go
package main

import "github.com/my/deployment/package"

func main() {
    // Deploy to 5% of users
    deployment.Rollout("new-feature", 5)
}
```

### **Blue-Green Deployment**

Blue-green deployment is a software release strategy that minimizes downtime and risk by running two identical production environments, often referred to as "blue" and "green."

Once the new version is deployed to the green environment, a series of comprehensive tests are conducted to ensure its functionality, performance, and integration meet expectations. This meticulous testing phase helps identify potential issues before exposing the new version to users. If the green environment proves stable and successful in the tests, traffic is seamlessly switched from the blue to the green environment, making the latter the new production environment.

To implement blue-green deployment successfully, organizations often leverage automated deployment tools and adhere to continuous integration practices to streamline the process and ensure a smooth transition between environments.

```go
package main

import (
	"fmt"
	"net/http"
	"os"
)

var isBlue bool

func main() {
	isBlue = true
	http.HandleFunc("/", handler)
	http.ListenAndServe(":8080", nil)
}

func handler(w http.ResponseWriter, r *http.Request) {
	if isBlue {
		fmt.Fprint(w, "Blue Environment - Old Behavior\n")
	} else {
		fmt.Fprint(w, "Green Environment - New Feature\n")
	}
    
    isBlue = !isBlue
}
func switchEnvironment() {
	isBlue = !isBlue
	fmt.Printf("Switched to %s environment\n", map[bool]string{true: "blue", false: "green"}[isBlue])
}

func init() {
	switchEnvironment()
}
```

The `handler` function serves different responses based on whether the environment is in the "blue" or "green" state. The `switchEnvironment` function allows you to manually switch between blue and green environments.

## Monitoring Like a Hawk

Once your canaries are out there singing, you need to listen carefully. Monitoring is key. Keep a close eye on your system's performance, error rates, and user feedback.

### Logging and Metrics

Enhance your logging game. Log relevant information about the new feature and monitor metrics that matter. Go's standard library makes this a breeze.

```go
// logger.go
package main

import "log"

func LogFeatureUsage(userID string) {
    log.Printf("User %s is using the new feature", userID)
}
```

### Error Tracking

Integrate error tracking tools. Identify and squash those bugs before they spread.

### Gradual Rollouts

Once the deployment of the new feature has been successful, we can enable the feature flag and all users can enjoy the new feature.

```go
// deployment.go
package main

import "github.com/your/deployment/package"

func main() {
    // Increase to 20% of users
    deployment.Rollout("new-feature", 20)
}
```

We can then monitor the usage of the new feature with our logs or analytics dashboards to see if users are adopting the feature and how they’re using it. The release of the feature has been completely independent of the app’s deployment!

### Rollbacks

Oops, something went wrong. No worries! Quickly roll back to the previous version using your deployment tool.

```go
// deployment.go
package main

import "github.com/your/deployment/package"

func main() {
    // Rollback to the previous version
    deployment.Rollback("new-feature")
}
```

## Monitoring and Observability Tools

Monitoring and observability are crucial components in overseeing canary releases and ensuring effective testing. These tools help teams gain insights into the performance, health, and behavior of applications during canary deployments. Here are some notable monitoring and observability tools that can aid in this process:

1. **Prometheus:**
    
    Prometheus is an open-source monitoring and alerting toolkit designed for reliability and scalability. It excels in collecting metrics, making it suitable for tracking the performance of canary releases. Prometheus is particularly valuable for its support of multi-dimensional data collection and querying.
    
2. **Grafana:**
    
    Grafana is a popular open-source platform for monitoring and observability that works seamlessly with data sources like Prometheus. It provides a customizable and interactive dashboard, allowing teams to visualize key metrics, logs, and traces. Grafana is valuable for gaining insights into application behavior during canary releases.
    
3. **Datadog:**
    
    Datadog is a cloud-based monitoring and analytics platform that offers comprehensive observability solutions. It provides real-time monitoring, logs, and traces, enabling teams to correlate data and troubleshoot issues effectively. Datadog supports canary releases by offering end-to-end visibility into application performance.
    
4. **New Relic:**
    
    New Relic is a cloud-based observability platform that provides insights into application performance, errors, and infrastructure. It offers APM (Application Performance Monitoring) capabilities and supports distributed tracing, making it valuable for monitoring canary releases and identifying any performance regressions.
    
5. **Istio:**
    
    Istio is an open-source service mesh that enhances visibility and control over microservices-based applications. It includes features like traffic management, security, and observability. Istio's observability tools, such as Prometheus integration and distributed tracing, are beneficial for monitoring canary releases in complex, microservices architectures.
    

When implementing canary releases, a combination of monitoring and observability tools ensures that teams have comprehensive insights into the impact of changes on application performance, allowing for effective testing and rapid identification of any issues that may arise during the release process.

## Challenges and Solution of Canary testing

### **Cons of Canary Testing**

Canary testing presents challenges such as the risk of false positives or negatives, where issues in the canary group may not accurately mirror the broader user base, leading to an incomplete understanding of the impact of changes. Managing multiple environments demands meticulous coordination, adding operational overhead. Striking a balance between the canary group's size and its representativeness is crucial, requiring constant attention. Improper execution can yield skewed results, hindering accurate assessments of changes on the overall system.

### **Pros of Canary Testing**

Canary testing minimizes software deployment risks by releasing changes to a limited user subset before wider adoption. This controlled exposure enables early identification and resolution of potential issues, reducing the impact on a larger audience and enhancing overall system stability. Valuable insights into real-world performance are gained, informing informed decisions based on actual user interactions. Aligned with continuous delivery practices, canary testing cultivates an iterative and cautious approach, fostering a culture of continuous improvement and innovation within development teams.

## Conclusion

The concept of canary testing, a strategic approach to introducing changes in a controlled manner by initially deploying them to a limited subset of users or systems.

One of the key benefits of canary testing lies in its ability to reduce risk. By allowing changes to be introduced gradually and observed within a confined environment, organizations can identify and address potential issues before they impact a larger audience. This controlled deployment method enhances the reliability of software updates and feature releases, providing a safety net for ensuring the stability and functionality of the new changes.

The successful implementation of canary testing hinges on careful planning. A well-thought-out strategy is crucial to ensure a smooth rollout process, encompassing considerations for the deployment mechanism, monitoring protocols, and contingency plans in case issues arise during the limited release. Additionally, effective communication is paramount. Transparent and clear communication among development, operations, and other involved teams establishes a collaborative environment where stakeholders are informed about the canary testing strategy, progress, and any necessary adjustments.