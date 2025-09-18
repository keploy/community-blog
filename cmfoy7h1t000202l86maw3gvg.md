---
title: "Mockito Spy: Your Complete Guide to Testing with Real Objects"
seoTitle: "Mockito Spy: Master Testing with Real Objects"
seoDescription: "Master Mockito Spy to elegantly handle real object testing, blending mock and actual behavior for precise control in your unit tests"
datePublished: Thu Sep 18 2025 05:06:29 GMT+0000 (Coordinated Universal Time)
cuid: cmfoy7h1t000202l86maw3gvg
slug: mockito-spy-your-complete-guide-to-testing-with-real-objects
canonical: https://keploy.io/blog/community/mockito-spy-your-complete-guide-to-testing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1756781068776/602fc0e7-715e-4618-9c3a-f6a684537e1e.png
tags: testing, mocking, test-automation, automation-testing, mockito

---

Hey! Quite a time right? Well we are back with another topic which is Mockito Spy. So you have ever been working with the unit tests and heard about this thing called Mocktio Spy, right? Maybe at this time, you’re even thinking what makes it different from other regular mocks or what this is spy actually?

Well, don’t worry about this stuff, in this blog I am going to walk you through everything about the Mockito spy and how it works and what actually you use it

Before knowing about what spy is, first try to understand what is Mockito [mock](https://keploy.io/test-data-generator) is and how it works!

## What is Mockito Mock?

![Mock](https://cdn.hashnode.com/res/hashnode/image/upload/v1756781996255/79313b2e-e04a-46aa-bcce-7280cba39062.png align="center")

A Mockito [Mock](https://keploy.io/test-data-generator) is an object created by the Mockito framework that analyzes the behavior of a real object or class. By using mocks, any user can control what methods return and verify interactions with these methods, isolating unit tests from external dependencies.

Here’s an example of how to mock an object using Mockito:

```java
import static org.mockito.Mockito.*;

public class UserServiceTest {

@Test

public void testGetUser() {

     // Create a mock of the UserRepository class

     UserRepository userRepository = mock(UserRepository.class);

    
         when(userRepository.getUserById(1)).thenReturn(new User(1, "Pratik Mahalle"))
   

     UserService userService = new UserService(userRepository);

     User user = userService.getUser(1);
   

     assertEquals("Pratik mahalle", user.getName());
}

}
```

## Importance of Mockito Mock

Below are the reasons why Mocking is important:

* **Isolation:** Mocks help isolate the tested unit by replacing real dependencies with controlled mock objects.
    
* **Behavior control**: Mocks allow the specification of exact behavior, such as method return values, exceptions, or interactions.
    
* **Test speed**: Mocks speed up unit tests since they do not involve expensive operations, like database calls or HTTP requests
    

## How does Mockito Mock work?

Mockito mocks a class or interface by creating a fake version that a user can control. When a method is called on a mock object, it returns default values unless explicitly told to return something else using `when(…).thenReturn(…)`.

### How to Use Mock – Steps

**Step 1**: Create the mock object by using `Mockito.mock(Class<T>.class)`.

**Step 2**: Use `when(…).thenReturn(…)` to specify what the mock should return.

**Step 3**: Call methods on the mock object in the test.

**Step 4**: Use `verify(mock).methodCall()` to confirm that methods were invoked.

## What Is a Mockito Spy?

![Mockito spy](https://cdn.hashnode.com/res/hashnode/image/upload/v1756781919964/b139ae34-2bb8-4eca-9f85-d9a26aca0054.png align="center")

A Mockito Spy is essentially a wrapper around a real object that allows you to monitor and partially control its behaviour during testing. Unlike mocks, which are completely fake objects, spies use actual instances of your classes while giving you the power to override specific methods when needed.

We can think of it like having a security camera watching your friend - most of the time, they act normally, but you can see everything they do and occasionally ask them to behave differently.

The main thing to remember is that spies execute real methods by default. You only stub the methods you specifically need to control, while everything else runs with genuine business logic.

Here is the example:

```java
import static org.mockito.Mockito.*;

public class UserServiceTest {

@Test

public void testSaveUser() {

       UserRepository realUserRepository = new UserRepository();

     UserRepository spyRepository = spy(realUserRepository);

     doReturn(false).when(spyRepository).save(any(User.class));

     // Test the method

     UserService userService = new UserService(spyRepository);

     boolean result = userService.saveUser(new User(1, "Ayush Singh"))

     assertFalse(result);

           }
}
```

## **What Is the Difference Between Spy and Mock?**

Here are some of the major differences between Mockito Mock and Spy:

| **Aspect** | **Mockito Mock** | **Mockito Spy** |
| --- | --- | --- |
| **Definition** | Creates a mock object of a class, which does not have real behavior unless explicitly defined. | Wraps an existing object, allowing both real and mocked behavior. |
| **Behavior** | Analyzes the behavior of a class. All methods return default values unless specified. | Executes real methods unless explicitly mocked. |
| **Use Case** | Ideal for isolating tests by managing dependencies. | Ideal for testing partial behaviors where some methods are real, and others are mocked. |
| **Stubbing** | Always need explicit stubbing for method behavior. | Allows partial stubbing along with real method calls. |
| **Object Type** | Can mock both interfaces and concrete classes. | Can spy on both interfaces and concrete classes. |
| **Default Behavior** | Returns default values like null, 0, or false unless specified. | Executes the real method unless explicitly mocked. |
| **Created by** | Created using **Mockito.mock(Class&lt;T&gt;.class).** | Created using **Mockito.spy(Object).** |

## When Should a Spy Be Used In Mockito?

You know, knowing when to reach for spies instead of mocks will definitely going to save you tons of time and frustration. Here are some scenarios of it:

**Perfect Spy Scenarios:**

* **Legacy code** **testing**: When you have established classes that work well but have external dependencies.
    
* **Partial behaviour control**: When you need real business logic for most methods but want to fake specific ones like database calls or API requests.
    
* **Complex object initialisation:** At the time of setting up, a full mock would require extensive stubbing of many methods.
    
* **Integration style** [**unit tests**](https://keploy.io/blog/community/what-is-unit-testing): When you want to test how multiple real methods work together while controlling specific dependencies.
    

**Stick with Mocks When:**

* You can easily isolate the unit being tested.
    
* The class possesses easy-to-reproduce behavior
    
* You want the best test performance.
    
* You're doing strict test-driven development
    

![When Should a Spy Be Used In Mockito?](https://cdn.hashnode.com/res/hashnode/image/upload/v1756782143478/6ccff02a-3b99-431e-b177-e01dea9aa91c.png align="center")

## **How to Use Mockito Spy in Practice**

Below are the steps on how you can start using Mockito Spy:

**Step 1**: Create the real object on which spy needs to be implemented.

**Step 2**: Use `Mockito.spy()` to wrap the real object.

**Step 3**: Use `doReturn(…).when(…)` to mock specific methods.

**Step 4**: Use `verify(spy).methodCall()` to verify interactions

## Mockito Spy: Learn It, Leverage It, Leave It

Here is what I think about the spies: they have incredibly useful in specific situations, but they shouldn’t be your default choice. You should learn how they work so you have them in your toolkit, use them but don’t hesitate to stick with simpler mocks when they do the job.

I have seen the developers go through the phases as I went it from most of the time - first avoiding spies completely, then using them everywhere, and finally settling into using them judiciously. The sweet spot is recognizing when spies genuinely make your tests better versus when they have just adding unnecessary complexity.

## Common Challenges of Using Mockito Spy

1. **Side Effects of Real Methods**
    
    As spies call real methods, you may inadvertently invoke file I/O, network requests, or database queries. So be careful what your unstubbed methods really do.
    
2. **Stubbing Gotchas**
    
    The worst error is to use `when().thenReturn()` in place of `doReturn().when().` This can result in real method calls being executed during setup time, and thus causing errant behavior or exceptions.
    
3. **Performance Implications**
    
    Spies are slower than mocks since they're executing actual code. For the average application, it does not matter, but in high-performance test suites, it does sum up.
    
4. **Overuse Temptation**
    
    Spies are simple to get too excited about and use everywhere since they feel more "real". They can cause tests to be difficult to understand and maintain.
    

## Best Practices of Using Mockito Spy

1. **Choose Spies Strategically**: Use them only when you genuinely need partial, real-world behaviour. If you find yourself stubbing most methods, a mock might be more cleaner.
    
2. **Always Use doReturn()**: This prevents accidental real method execution during stubbing setup.
    
3. **Document Your Reasoning**: Leave comments explaining why you chose a spy over a mock. It helps future contributors and maintainers understand your testing approach in the process.
    
4. **Watch for Side Effects**: Be confident about which methods you're leaving unstubbed and ensure they won't cause problems in your test environment.
    
5. **Keep Tests Focused**: Even with real behaviour, each test should verify one specific aspect of your code's functionality.
    
6. **Combine with Verification**: Take advantage of spies' ability to verify both stubbed and real method interactions
    

## Conclusion

Mockito Spy is like that swiss Army knife in testing toolkit which is super handy when you need it, but you would not use it to eat dinner. It bridges the gap between pure mocks and real objects, giving you the best of both worlds when you need partial stubbing.

Always remember - use spies when you need real behaviour with selective control, stick to mocks for clean isolation, and always think about what you’re actually trying to test.

## FAQ

### 1\. **Can I spy on interfaces?**

Nope! Spies need concrete classes with real implementations. Use mocks for interfaces.

### **2\. What happens if I don't stub a method on a spy?**

The real method runs with real behavior. That's the whole point of spies!

### **3\. Can I spy on static methods?**

With regular Mockito, no. You'd need PowerMock or Mockito's newer inline mock maker.

### **4\. Are spies slower than mocks?**

Yes, slightly, because they're wrapping real objects. But unless you're running thousands of tests, you won't notice.

### **5\. Should I use spies for new code?**

Generally, no. Design new code to be easily testable with regular mocks. Spies are better for legacy code or special cases