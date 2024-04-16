---
title: "Write Clean and Efficient Unit Tests in Go"
seoTitle: "Efficient and Maintainable Table driven Unit Tests in Go"
seoDescription: "Table driven tests, also known as parameterized tests, have became very popular over the past few years, due to their ability to eliminate repetition."
datePublished: Sun Apr 14 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clv1xlbx9000w07l64ig660vh
slug: write-clean-and-efficient-unit-tests-in-go
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713244457099/b380b048-f33f-4d21-b63c-83497c0ca9d3.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1713244521122/0cbffecd-34ea-4967-aa54-d168e511a017.png
tags: unit-testing, golang, testing, go-testing, table-driven-testing

---

*Table driven tests*, also known as parameterized tests, have became very popular over the past few years, due to their ability to eliminate repetition. Table driven tests make it quite a bit easier to re-use the same values for different sets of tests by just moving the table outside of the scope of the test function. Different tests may benefit from the same input, and each test may have completely different configration, concurrency etc...

## What is a Table Driven **Unit Test** ?

Well, think of it as a systematic way of testing your code by providing a set of inputs and expected outputs in a structured table format. Instead of writing individual test cases for each input-output pair, we organize them neatly in a table, making it easier to manage and maintain our tests.

Each row in the table represents a separate test case, and the columns represent different aspects or parameters of the test, such as inputs, expected outputs, and any other relevant conditions.

Here's a simplified example of what a table-driven test might look like for a function that calculates the square of a number:

```go
package main

import (
    "testing"
)

func Add(a, b int) int {
    return a + b
}

func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        inputA   int
        inputB   int
        expected int
    }{
        "10+3": {
            inputA:        10,
            inputA:        3,
            expected: 13,
        },
        "2+6": {
            inputA:        2,
            inputA:        6,
            expected: 8,
        },
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.inputA, tt.inputB)
            if result != tt.expected {
                t.Errorf("got %d, want %d", result, tt.expected)
            }
        })
    }
}
```

Here, once we have declared the test cases via the `tests` map, the test function iterates over all the map entries, i.e. the test cases. The `Add` function is called in each iteration using the input arguments of the current test case. If the actual result doesn’t match the result expected by the test case, the test fails with an error message.

### **Why Use Table-Driven Tests?**

Now, you might be wondering, why bother with table-driven tests? Here are a few reasons:

1. **Clarity:** By organizing test cases in a table, it's easier to see all the inputs and expected outputs at a glance. This makes it simpler to understand what the test is doing.
    
2. **Scalability:** As your codebase grows, you'll likely have more test cases to manage. Table-driven tests make it easier to add new test cases without cluttering your test code.
    
3. **Maintainability:** Since test cases are organized in a structured format, it's easier to update or modify them when needed.
    

### **Maps and slices, which to choose?**

Usually, test cases contain a name field to uniquely identify and describe the test case, and these are stored in slice.

```go
    tests := []struct {
        name     string
        inputA   int
        inputB   int
        expected int
    }
```

Although, slices are better approach when the test cases are simple, It's is preferred to use maps for storing when a lot of complex test cases start occupying too much space. Other benefit of using maps instead of the slice is that, it is an undefined iteration order. This means that test cases might be executed in a different order each time, which could expose issues in your test setup.

```go
    tests := map[string]struct {
        inputA   int
        inputB   int
        expected int
    }
```

It is also an issue because, if your tests expect certain conditions to be met in a specific order, using maps for table-driven tests might not be the best choice because the iteration order is not guaranteed. It's essential to consider this when designing your test setup to ensure reliable and consistent testing results.

Solution to this that the order of test cases are maintained explicitly. This can be done by using an `ordered slice of structs` instead of a map.

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        inputA   int
        inputB   int
        expected int
    }{
        {"Test case 1", 1, 2, 3},
        {"Test case 2", -1, 1, 0},
        {"Test case 3", 0, 0, 0},
    }

    for _, tt := range tests {
        tt := tt // capture range variable
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.inputA, tt.inputB)
            if result != tt.expected {
                t.Errorf("got %d, want %d", result, tt.expected)
            }
        })
    }
}
```

Here, we use a slice of structs to maintain the order of test cases. Inside the loop, we create a local copy of `tt` using `:=` to ensure that each iteration has its own copy of the test case. This prevents any unintended sharing of state between iterations.

By using an ordered slice instead of a map, we ensure that the test cases are executed in the defined order, addressing the concern of non-deterministic behavior introduced by map iteration. This approach provides more reliability and consistency in test execution.

### Logging

When a comparison between an actual value and an expected value reveals a mismatch, there are two ways to handle it. Either we log an error message using Errorf or we terminate the test using Fatalf.

In the first scenario, we log the error but allow the test to continue with subsequent checks. The latter option stops the entire test immediately upon encountering a failure. It's important to use these options thoughtfully. Generally, it's recommended to log the failure and continue with the test using Errorf. Only if proceeding with the test wouldn't make sense due to failed preconditions, should Fatalf be used to halt the test immediately.

For instance, if the lengths of two slices, actual and test.expected, don't match, we immediately fail the test with Fatalf:

```go
if len(actual) != len(tt.expected) {
    t.Fatalf("lengths don't match: expected %v, got %v", len(tt.expected), len(actual))
}
```

Assuming the lengths are the same, we compare the values within the slices one by one. If a mismatch occurs, we log an error using Errorf but continue with the loop:

```go
for i, value := range tt.expected {
    if actual[i] != value {
        t.Errorf("values at index %d don't match: expected %v, got %v", i, value, actual[i])
    }
}
```

### Helper Structs

To manage complex test cases with many parameters or dependencies, we can use helper structs. These structs store input parameters and expected results separately:

```go
func TestAdd(t *testing.T) {
    type Given struct {
        inputA int
        inputB int
    }

    type Expected struct {
        result int
    }

    tests := map[string]struct{
        given    Given
        expected Expected
    }{
    }
}
```

By defining helper structs within the test function, we can keep the test cases clear and manageable. This approach is particularly useful for complex test cases.

Additionally, if a function becomes too complex, it's a sign that it may be doing too much. In such cases, it's beneficial to split the function into smaller, more manageable pieces with clearly defined responsibilities. This simplifies both the function itself and its associated test cases.

## Conclusion

Table driven tests are a popular model for testing in Go and a clear improvement to traditional testing of one function per case. As it offers a convenient way to test functions with various inputs. Since, the approach of "table" are easy and better to scale, most language testing frameworks have adopted some concept of dataprovider.

They are great when you've a simply set of primitives as inputs, and you get out something simple in return. For anything more complex. use of a load of `t.Runs` inside a TestMyFunc, becomes necessary. Personally, a mix is fine, since it allows to test application in more than one scenario as well as are easy to maintain and document.

## FAQ's

### Are having dependencies are not a total blocker to a table-driven test ?

You can mock dependencies as additional fields in test structs, enabling comprehensive testing of functions with external dependencies in table-driven tests.

### How can I manage complex test cases efficiently?

Utilize helper struct more, helper structs separate input parameters and expected results, enhancing test case clarity and manageability by structuring data into distinct categories for better organization and readability.