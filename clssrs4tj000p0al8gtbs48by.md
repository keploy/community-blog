---
title: "Mock vs Stub vs Fake: Understand the difference"
seoDescription: "Learn the differences between mocks, stubs, and fakes to improve your software testing strategy. Discover how to use these test doubles effectively to enhance code quality, speed up testing, and ensure reliable results."
datePublished: Tue Feb 13 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clssrs4tj000p0al8gtbs48by
slug: mock-vs-stub-vs-fake-understand-the-difference
canonical: https://keploy.io/blog/community/mock-vs-stub-vs-fake-understand-the-difference
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707585234655/2ea8b5cc-0681-437e-b39f-be378de2bd24.png

---

## Introduction

Testing software is like putting it through a series of challenges to make sure it's tough enough for real-world use. Whether we're testing each piece individually (unit testing) or how they all work together (integration testing), we need to be prepared for different situations.

Sometimes, testing is tricky.There are times, when we need to isolate parts of our code from their dependencies. Also When we examine how different pieces of our application interact with each other.That's where tools like Fake, Mock, and Stub come into the picture.

In this Article, We'll explore What about them and compare Mock vs Stub vs Fake.

## Overview of Mock vs Stub vs Fake

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707286226829/b92297d7-cbd7-4457-9a5b-2b81735b9937.png align="center")

Fakes are a way for us to simulate objects and method functionality in our code without actually depending on a real object or method.

Stubs are a type of fake that quietly fakes behavior and can return a predefined expected value, while a mock is a type of fake that we can monitor to make sure that a certain method was called an expected amount of times or with an expected set of parameters.

In conclusion, while all three— Fake, Mock, and Stub—serve as valuable tools in software testing, their differences lie in their roles. In the following sections we'll explore more about this.

## What is Mock Testing?

Mock testing is a testing technique that is used in software development to simulate the behavior of components that a software module depends on.

![MockServer](https://www.mock-server.com/images/system_under_test_with_mockserver.png align="left")

Let's say we have a function that sends an email to a user when they register on a website. In our unit test for this function, we want to ensure that the email is sent correctly without actually sending an email every time we run the test.

Instead, we can use a mock email service that replicates sending emails. It helps us to check if our program is working correctly or not!

## What are Stubs?

In simple words, A Stub is a simplified version of a software component used in testing that imitates the behavior of a service or object.

Suppose, We're developing a software application that relies on a database connection, but the database system isn't ready yet.

Now we'll create a stub function that simulates the behavior of the database connection without actually performing any database operations. This stub function will return dummy data or predefined responses to mimic the expected behavior of the real database connection.

Thus, we can work on the testing part without thinking much about the depenencies.

![Stub vs. Mock - Must-Know Top Differences Between Them](https://images.prismic.io/turing/65a53fea7a5e8b1120d58939_mock_vs_stub_4ab4d1c297.webp?auto=format,compress align="left")

## What is Fake?

Just like stubs, a fake is a simplified version of a real component used in testing to mimic its behavior. Fakes typically have working implementations of all methods but may simplify complex behaviors or dependencies to make testing easier.

For example, a fake database might store data in memory instead of a physical database, or a fake HTTP client might return predefined responses instead of making actual network requests. Fakes help isolate code for testing and make tests faster and more predictable.

## **Why Do We Need Mock, Stub and Fake?**

When the complexity of the project increases, the need of Fake, Mock and Stub increases.Testing applications with intricate dependencies can be challenging for testers.

Mock, Stub and Fake offer valuable tools for simplifying this process,It enables more effective testing practices and improves the quality and reliability of software applications.

By understanding the distinctions between Mock vs Stub vs Fake & leveraging them effectively, we can make test our softwares easily: -

| **Aspect** | **Mocks** | **Stubs** | **Fakes** |
| --- | --- | --- | --- |
| **Definition** | Simulate and monitor the behavior of dependencies. | Provide predefined responses for dependencies. | Provide simplified, yet working, implementations. |
| **Purpose** | Verify interactions and check specific calls. | Return specific values for testing. | Simplify complex behaviors for easier testing. |
| **Implementation** | Can verify method calls, arguments, and invocation. | Return canned responses without behavior logic. | Have working implementations but with simplifications. |
| **Use Case Example** | Testing if an email service method is called with correct arguments. | Providing dummy data from a database query. | Using an in-memory database instead of a real one. |
| **Behavior** | Can assert the number and nature of calls made. | Passive; returns data without asserting behavior. | Active; performs actions but in a simplified manner. |
| **Complexity** | More complex, as they need to check and assert interactions. | Less complex, just need to return predefined values. | Medium complexity, as they involve actual logic. |
| **Dependency** | Requires setup and verification steps. | Requires setting up return values. | Requires implementation of methods. |
| **Performance** | Can be slower due to additional verification steps. | Fast, as they return static data. | Generally fast but could vary based on implementation. |
| **Example Frameworks** | Mockito, EasyMock | JUnit | FakeItEasy, In-memory databases |
| **Suitability** | Best for verifying interactions and side effects. | Best for returning specific data needed for a test. | Best for scenarios where full behavior is needed but can be simplified. |

## Real-Life Use Cases and Code Samples

### Use Case: E-commerce Application

Imagine you're working on an e-commerce application with the following functionalities:

- **User Registration:** Sends a welcome email when a user registers.
- **Order Processing:** Interacts with a payment gateway and updates inventory.

We’ll use Mocks, Stubs, and Fakes to test these functionalities.

### 1. Mocks


**Scenario:** You want to ensure that an email is sent when a user registers, and you need to verify that the email service is called with the correct parameters.

**Code Sample:**

```java
// EmailService.java - Interface for email service
public interface EmailService {
    void sendEmail(String recipient, String subject, String body);
}

// UserService.java - Service that uses EmailService
public class UserService {
    private EmailService emailService;

    public UserService(EmailService emailService) {
        this.emailService = emailService;
    }

    public void registerUser(String email) {
        // User registration logic
        emailService.sendEmail(email, "Welcome!", "Thank you for registering.");
    }
}

// UserServiceTest.java - Test class using Mockito for mocking
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

public class UserServiceTest {
    @Test
    public void testRegisterUserSendsEmail() {
        // Create a mock of EmailService
        EmailService mockEmailService = Mockito.mock(EmailService.class);
        UserService userService = new UserService(mockEmailService);

        // Call the method under test
        userService.registerUser("user@example.com");

        // Verify that sendEmail was called with correct arguments
        Mockito.verify(mockEmailService).sendEmail("user@example.com", "Welcome!", "Thank you for registering.");
    }
}
```
**Explanation:** In this example, we use Mockito to create a mock of EmailService. We verify that the sendEmail method is called with the correct arguments when the registerUser method is invoked.


### 2. Stubs

**Scenario:** During development, you need to test the order processing logic but the payment gateway is not yet integrated. You use a stub to simulate the payment gateway’s behavior.

**Code Sample:**

```java
// PaymentGateway.java - Interface for payment gateway
public interface PaymentGateway {
    boolean processPayment(double amount);
}

// OrderService.java - Service that uses PaymentGateway
public class OrderService {
    private PaymentGateway paymentGateway;

    public OrderService(PaymentGateway paymentGateway) {
        this.paymentGateway = paymentGateway;
    }

    public boolean placeOrder(double amount) {
        return paymentGateway.processPayment(amount);
    }
}

// OrderServiceTest.java - Test class using a stub
import org.junit.jupiter.api.Test;

public class OrderServiceTest {
    @Test
    public void testPlaceOrderWithStub() {
        // Create a stub for PaymentGateway
        PaymentGateway stubPaymentGateway = new PaymentGateway() {
            @Override
            public boolean processPayment(double amount) {
                // Return a fixed response
                return amount > 0;
            }
        };
        OrderService orderService = new OrderService(stubPaymentGateway);

        // Test with a positive amount
        boolean result = orderService.placeOrder(100.0);
        assert result; // Should be true

        // Test with a zero or negative amount
        result = orderService.placeOrder(0.0);
        assert !result; // Should be false
    }
}
```
**Explanation:** In this example, we create a stub of PaymentGateway that always returns true for positive amounts and false otherwise. This allows us to test the OrderService logic without relying on a real payment gateway.


### 3. Fakes

**Scenario:** You want to test the application’s integration with an in-memory database rather than a real database for faster and simpler tests.

**Code Sample:**

```java
// Database.java - Interface for database operations
public interface Database {
    void saveUser(String username, String password);
    String getUser(String username);
}

// InMemoryDatabase.java - Fake implementation of Database
import java.util.HashMap;
import java.util.Map;

public class InMemoryDatabase implements Database {
    private Map<String, String> data = new HashMap<>();

    @Override
    public void saveUser(String username, String password) {
        data.put(username, password);
    }

    @Override
    public String getUser(String username) {
        return data.get(username);
    }
}

// UserService.java - Service that uses Database
public class UserService {
    private Database database;

    public UserService(Database database) {
        this.database = database;
    }

    public void registerUser(String username, String password) {
        database.saveUser(username, password);
    }

    public String getUserPassword(String username) {
        return database.getUser(username);
    }
}

// UserServiceTest.java - Test class using InMemoryDatabase
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

public class UserServiceTest {
    @Test
    public void testUserRegistrationWithFake() {
        // Create a fake in-memory database
        Database fakeDatabase = new InMemoryDatabase();
        UserService userService = new UserService(fakeDatabase);

        // Register a user
        userService.registerUser("user1", "password123");

        // Retrieve and verify user information
        String password = userService.getUserPassword("user1");
        assertEquals("password123", password);
    }
}
```
**Explanation:** In this example, InMemoryDatabase is a fake implementation of the Database interface. It mimics a real database’s behavior but stores data in memory, which makes testing faster and more straightforward.

## **How Does Keploy help with Mocks, Stubs and Fake?**

Keploy, is an popular testing tool which makes testing easier by automatically creating mocks, stubs, and fakes. Keploy generates mocks from real interactions, so you don’t have to create them manually. Stubs are simpler—they return predefined responses and are useful for testing specific parts of your application without needing the actual service. Keploy helps you set up these stubs quickly, making your testing process faster and more efficient. Fakes are like mocks but mimic the behavior of real components more closely, allowing you to test different scenarios without needing the real components.

By using Keploy, you can easily integrate these mocks, stubs, or fakes into your existing testing setup. This means you don’t need to spend a lot of time and effort creating and managing test data. Keploy’s automated process saves you time, allowing you to focus on improving your application. Plus, the dynamic and realistic data provided by Keploy ensures that your tests are accurate and reliable. Overall, Keploy simplifies the testing process, helping you deliver robust and reliable software more efficiently.

## Conclusion

In short, these tools are essential for ensuring that software functions correctly under various conditions and dependencies, making the testing process more robust and reliable. By understanding their differences and appropriate use cases, developers can choose the right tool for the job and improve their testing efficiency.

If you found this blog post helpful, please consider sharing it with others who might benefit. To sponsor my work, please visit: [Arindam's Sponsor Page](https://arindam1729.hashnode.dev/sponsor) and explore the various sponsorship options.

Connect with me on [Twitter](https://twitter.com/intent/follow?screen_name=Arindam_1729), [LinkedIn](https://www.linkedin.com/in/arindam2004/), [Youtube](https://www.youtube.com/channel/@Arindam_1729) and [GitHub](https://github.com/Arindam200).

## Frequently Asked Questions

### When should I use mocks instead of stubs?

Use mocks when you need to verify that specific methods are called with particular arguments and ensure that certain interactions occur. Mocks are ideal for testing interactions and side effects.

### Can fakes be used in both unit and integration testing?

Yes, fakes can be used in both unit and integration testing. They are particularly useful in unit tests to isolate the code under test and in integration tests to simulate parts of the system that are not yet implemented or are too complex to use directly.

### How do stubs help in testing when the actual service is not available?

Stubs simulate the behavior of the actual service by returning predefined responses. This allows testing to proceed without relying on the actual service, which may not be available or ready, ensuring that the code can still be tested in isolation.

### How do mocks, stubs, and fakes impact the performance of tests?

* **Mocks**: Can slow down tests due to the additional steps needed for setting up and verifying interactions.
    
* **Stubs**: Generally fast, as they return static, predefined data without complex logic.
    
* **Fakes**: Usually fast but can vary based on their implementation. They perform actual logic but in a simplified manner, which can sometimes be more performant than using real dependencies.
    

### How does Keploy help with creating mocks?

Keploy automatically generates mock data based on recorded interactions, eliminating the need for manual mock creation and simplifying the testing process. Since the keploy provides dynamic and realistic mock data, it helps to create accurate and reliable test scenarios that closely mimic real-world conditions.



![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707304021607/2bf942ef-b3a2-4db2-8699-4573c661cac1.png align="center")
