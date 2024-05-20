---
title: "Why I Switched to Table Driven Testing approach in Go"
seoTitle: "A Guide to Table Driven Unit Tests in Go"
seoDescription: "Table driven tests, also known as parameterized tests, have became very popular over the past few years, due to their ability to eliminate repetition."
datePublished: Sun Apr 14 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clv1xlbx9000w07l64ig660vh
slug: write-clean-and-efficient-unit-tests-in-go
canonical: https://keploy.io/blog/community/write-clean-and-efficient-unit-tests-in-go
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713244457099/b380b048-f33f-4d21-b63c-83497c0ca9d3.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1713244521122/0cbffecd-34ea-4967-aa54-d168e511a017.png
tags: unit-testing, golang, testing, go-testing, table-driven-testing

---

Unit Testing, specifically table driven tests, have gained significant popularity in recent years due to their ability to eliminate repetition and enhance test clarity and maintainability. By organizing test cases into a structured format, table driven tests provide a systematic way to reuse the same values across different unit testing scenarios efficiently.

## What is Table Driven Tests in Unit Testing?

Table driven tests involve creating a table of inputs and expected outputs, where each row represents a separate test case and columns represent different parameters of the test, such as inputs, expected outputs, and any other relevant conditions. This approach makes it easier to manage and maintain tests by centralizing test data, which is particularly beneficial in unit testing.

### Example of a Table Driven Test in Unit Testing with Go

Here's a simplified example demonstrating a table driven test for a function that calculates the sum of two numbers:

```go
package main

import (
    "testing"
)

// Function to be tested in unit testing
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
        {"10+3", 10, 3, 13},
        {"2+6", 2, 6, 8},
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

#### Previous Model of Testing

```go
func TestAdd(t *testing.T) {
    result := Add(10, 3)
    if result != 13 {
        t.Errorf("got %d, want 13", result)
    }

    result = Add(2, 6)
    if result != 8 {
        t.Errorf("got %d, want 8", result)
    }
}
```

table-driven tests provide a more structured, scalable, and maintainable approach compared to the traditional method of writing individual test cases. This approach is particularly beneficial for managing a large number of test cases and for ensuring the clarity and readability of your unit tests.

### Benefits of Table Driven Tests in Unit Testing

1. **Clarity**: Organizing test cases in a table format makes it easy to see all inputs and expected outputs at a glance.
    
2. **Scalability**: Simplifies adding new test cases as the codebase grows without cluttering the unit testing functions.
    
3. **Maintainability**: Structured format allows for easier updates and modifications in unit testing.
    

Sure, here's a comparison table that highlights the differences between table-driven tests and the old method of testing:

| **Aspect** | **Table-Driven Tests** | **Old Method of Testing** |
| --- | --- | --- |
| **Structure** | Organized in a table format with rows representing test cases and columns representing parameters | Individual test cases written separately, often resulting in repetitive code |
| **Clarity** | High - Inputs and expected outputs are easily visible and organized | Lower - Test logic is mixed with test data, making it harder to see the big picture |
| **Scalability** | High - Easy to add new test cases by adding rows to the table | Lower - Adding new test cases often requires duplicating code blocks |
| **Maintainability** | High - Easier to update and modify test cases in a centralized table | Lower - Updates require changes in multiple places, increasing the risk of errors |
| **Code Duplication** | Minimal - Test logic is separated from test data | High - Each test case may repeat similar logic |
| **Readability** | High - Clearly defined inputs, outputs, and test names in a structured format | Lower - Test cases can be harder to read due to intermingled logic and data |
| **Flexibility** | High - Can easily handle multiple test cases with different parameters | Lower - Each test case needs to be manually written, making it less flexible |
| **Complexity Handling** | Good - Can use helper structs and mocks for complex scenarios | Moderate - Complex scenarios can lead to complicated and hard-to-maintain test code |
| **Order of Execution** | Defined - Using slices, order is maintained explicitly | Not always defined - Each test function executes separately, order depends on function organization |
| **Parallel Execution** | Easy - Can use `t.Parallel` within each test case | Less straightforward - Requires more setup to achieve parallel execution |

### Structuring Table Driven Tests in Unit Testing

#### Using Slices

For simple unit testing cases, slices of structs are a common choice:

```go
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
```

#### Using Maps

For more complex unit testing cases or when iteration order is not important, maps can be used:

```go
tests := map[string]struct {
    inputA   int
    inputB   int
    expected int
}{
    "Test case 1": {1, 2, 3},
    "Test case 2": {-1, 1, 0},
    "Test case 3": {0, 0, 0},
}
```

However, because map iteration order is not guaranteed, using an ordered slice of structs is often preferred to ensure consistent unit testing execution order.

### Handling Errors and Logging in Unit Testing

Proper error handling and logging are crucial for effective unit testing. Use `Errorf` to log errors without stopping the test and `Fatalf` to terminate the test immediately if a critical failure occurs.

```go
if len(actual) != len(tt.expected) {
    t.Fatalf("lengths don't match: expected %v, got %v", len(tt.expected), len(actual))
}

for i, value := range tt.expected {
    if actual[i] != value {
        t.Errorf("values at index %d don't match: expected %v, got %v", i, value, actual[i])
    }
}
```

### Managing Complex Unit Testing Cases

For complex unit testing cases with many parameters or dependencies, use helper structs to keep the tests clear and manageable:

```go
type Given struct {
    inputA int
    inputB int
}

type Expected struct {
    result int
}

tests := map[string]struct {
    given    Given
    expected Expected
}{
    "Test case 1": {Given{1, 2}, Expected{3}},
    "Test case 2": {Given{-1, 1}, Expected{0}},
}
```

### Parallel Execution of Unit Testing Cases

To speed up test execution, especially when tests are independent of each other, use `t.Parallel` in your unit testing:

```go
for _, tt := range tests {
    tt := tt // capture range variable
    t.Run(tt.name, func(t *testing.T) {
        t.Parallel()
        result := Add(tt.inputA, tt.inputB)
        if result != tt.expected {
            t.Errorf("got %d, want %d", result, tt.expected)
        }
    })
}
```

## Conclusion

Table driven tests are a powerful model for unit testing in Go, offering a structured, scalable, and maintainable approach to handle various input scenarios. This method, while popular in Go, can also be applied to other languages, such as TypeScript, to leverage its benefits in unit testing.

By using table driven tests, developers can ensure their code is robust, maintainable, and easy to understand, significantly enhancing the overall quality of their unit testing and software projects.

## Frequently Asked Questions

### Are having dependencies are not a total blocker to a table driven test ?

You can mock dependencies as additional fields in test structs, enabling comprehensive testing of functions with external dependencies in table driven tests.

### How can I manage complex test cases efficiently?

Utilize helper struct more, helper structs separate input parameters and expected results, enhancing test case clarity and manageability by structuring data into distinct categories for better organization and readability.

### Can we use Table Driven test in Golang only?

While table driven tests are most popular with Go, TypeScript is also a perfect language for them. Being a strongly-typed superset of JavaScript, TypeScript allows us to explicitly define our input and expected types, an important feature to have when we are writing our tests.

### **How do table driven tests improve the process of unit testing?**

Table driven tests streamline the unit testing process by organizing test cases in a structured format. This approach makes it easy to manage and maintain tests by centralizing test data. Each row in the table represents a separate test case with specific inputs and expected outputs. This organization enhances clarity, scalability, and maintainability, allowing developers to add new test cases without cluttering the test code and making it simpler to understand and update the tests as needed.