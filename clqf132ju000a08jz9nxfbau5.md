---
title: "Creating the Balance Between End-to-End and Unit Testing"
datePublished: Tue Dec 19 2023 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clqf132ju000a08jz9nxfbau5
slug: creating-the-balance-between-end-to-end-and-unit-testing
canonical: https://keploy.io/blog/community/creating-the-balance-between-end-to-end-and-unit-testing
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702978978336/290b9a01-70b1-4d98-a16b-39baa36a76c4.png

---

In the dynamic landscape of software development, the approach to testing has evolved significantly. One of the debates that often surfaces is the choice between end-to-end (E2E) testing and unit testing, particularly when it comes to testing in a live production environment. In this blog, we'll explore the rationale behind Testing in Production and why striking a balance between E2E and unit testing is crucial for ensuring robust software applications.

## **Understanding Testing in Production**

Testing in Production is a paradigm shift that recognizes the need to validate the behaviour of software in a real-world environment. Traditional testing methodologies, such as unit testing in isolation or staging environments, may not capture all the nuances and interactions that occur in the actual production setting.

1. **Real-world Scenarios:** Testing in Production allows developers and testers to observe how the software behaves under real-world conditions. It provides insights into performance, scalability, and interactions with other services or components that might not be accurately simulated in controlled testing environments.
    
2. **Incremental Deployment:** With modern deployment practices like continuous delivery and continuous deployment, software changes are rolled out incrementally. Testing in Production supports these practices by validating each incremental change in the live environment, reducing the risk of unforeseen issues when deploying larger updates.
    

For better understanding how to we can balance between end-to-end and unit testing while testing in production, let's create a sample application and create both unit-test as well as e2e test for it. Let's create our `task.go` file: -

```go
// task.go
package main

import (
    "database/sql"
    "encoding/json"
    "log"
    "net/http"

    _ "github.com/lib/pq"
)

type Task struct {
    ID    int    `json:"id"`
    Title string `json:"title"`
    Done  bool   `json:"done"`
}

func main() {
    // Connect to the database
    db, err := sql.Open("postgres", "postgres://postgres:postgres@localhost:5432/taskdb?sslmode=disable")
    if err != nil {
        log.Fatal("Error connecting to the database:", err)
    }
    defer db.Close()

    // Define HTTP endpoints
    http.HandleFunc("/tasks", getTasksHandler(db))
    http.HandleFunc("/tasks/create", createTaskHandler(db))
    http.HandleFunc("/tasks/update", updateTaskHandler(db))
    http.HandleFunc("/tasks/delete", deleteTaskHandler(db))

    // Start the HTTP server
    log.Println("Server started on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}

func getTasksHandler(db *sql.DB) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
        // Fetch tasks from the database
        rows, err := db.Query("SELECT id, title, done FROM tasks")
        if err != nil {
            http.Error(w, "Failed to fetch tasks", http.StatusInternalServerError)
            return
        }
        defer rows.Close()

        // Convert rows to Task objects
        tasks := make([]Task, 0)
        for rows.Next() {
            var task Task
            if err := rows.Scan(&task.ID, &task.Title, &task.Done); err != nil {
                http.Error(w, "Failed to scan tasks", http.StatusInternalServerError)
                return
            }
            tasks = append(tasks, task)
        }

        // Convert tasks to JSON and write to response
        w.Header().Set("Content-Type", "application/json")
        json.NewEncoder(w).Encode(tasks)
    }
}
```

The above application is a simple REST API for managing tasks. We have endpoints for creating, updating, deleting, and fetching tasks.In terms of external dependency our application will use a PostgreSQL database to store tasks.

## **Understanding Unit Testing**

Unit Testing focuses on individual units of code, usually functions or methods. It's like dissecting your application into bite-sized chunks and testing each one in isolation.

Since you're testing small units of code, it's fast, you can run hundreds or even thousands of tests in the blink of an eye. Plus, it's less prone to flakiness compared to E2E testing, making it more reliable in the long run.

### Types of Unit Testing

There are different types of unit testing, including two main types:

* White-box testing
    
* Black-box testing
    

White-box testing ensures examining the internal structure and code of the unit being tested, while black-box testing focuses on the unit's external behavior without considering its internal implementation.

Let's create a Unit test file named `task_test.go` for the sample application we created earlier: -

```go
// task_test.go
package main

import (
    "testing"
    "net/http"
    "net/http/httptest"
    "strings"
)

func TestGetTasksHandler(t *testing.T) {
    // Create a mock database
    db, _ := setupMockDB()

    // Create a request to the handler
    req, err := http.NewRequest("GET", "/tasks", nil)
    if err != nil {
        t.Fatal(err)
    }

    // Create a response recorder
    rr := httptest.NewRecorder()

    // Call the handler function
    handler := http.HandlerFunc(getTasksHandler(db))
    handler.ServeHTTP(rr, req)

    // Check the response status code
    if status := rr.Code; status != http.StatusOK {
        t.Errorf("handler returned wrong status code: got %v want %v",
            status, http.StatusOK)
    }

    // Check the response body
    expected := `[{"id":1,"title":"Task 1","done":false},{"id":2,"title":"Task 2","done":true}]`
    if rr.Body.String() != expected {
        t.Errorf("handler returned unexpected body: got %v want %v",
            rr.Body.String(), expected)
    }
}
```

The unit test we just created verifies that the `getTasksHandler()` function in `task.go` behaves correctly when called with a **GET** request to fetch tasks from the database. It checks both the **HTTP** status code and the response body to ensure they match the expected values. If any of these checks fail, the test will report an error.

But unit testing isn't without its limitations. While it's great for catching bugs at the granular level, it can't guarantee that everything will work seamlessly when you put all the pieces together. That's where end-to-end testing comes into play.

## **The Role of End-to-End Testing**

End-to-End testing involves validating the entire application workflow, from the user interface to the backend. While unit testing focuses on isolated components, E2E testing ensures that the entire system functions seamlessly.

1. **User-Centric Validation:** E2E testing is user-centric, simulating user interactions and scenarios. This ensures that the software meets user expectations and behaves as intended from a user's perspective.
    
2. **Integration Testing:** E2E testing inherently includes integration testing, verifying that different components of the system work together seamlessly. This is crucial for identifying issues that may arise when various modules interact.
    

```go
package main_test

import (
    "bytes"
    "encoding/json"
    "net/http"
    "net/http/httptest"
    "testing"
)

func TestTaskAPIE2E(t *testing.T) {
    // Set up a mock database
    db, _ := setupMockDB()

    // Create a new HTTP server using the main application's handler
    server := httptest.NewServer(http.HandlerFunc(getTasksHandler(db)))
    defer server.Close()

    // Test creating a new task
    createTaskReq, _ := http.NewRequest("POST", server.URL+"/tasks/create", bytes.NewBufferString(`{"title":"New Task"}`))
    createTaskResp, err := http.DefaultClient.Do(createTaskReq)
    if err != nil {
        t.Fatalf("Failed to create task: %v", err)
    }
    defer createTaskResp.Body.Close()

    // Check if creating task returns a successful response
    if createTaskResp.StatusCode != http.StatusOK {
        t.Errorf("Expected status code %d, got %d", http.StatusOK, createTaskResp.StatusCode)
    }

    // Test fetching all tasks
    getTasksReq, _ := http.NewRequest("GET", server.URL+"/tasks", nil)
    getTasksResp, err := http.DefaultClient.Do(getTasksReq)
    if err != nil {
        t.Fatalf("Failed to get tasks: %v", err)
    }
    defer getTasksResp.Body.Close()

    // Check if fetching tasks returns a successful response
    if getTasksResp.StatusCode != http.StatusOK {
        t.Errorf("Expected status code %d, got %d", http.StatusOK, getTasksResp.StatusCode)
    }

    // Decode response body into slice of Task objects
    var tasks []Task
    if err := json.NewDecoder(getTasksResp.Body).Decode(&tasks); err != nil {
        t.Fatalf("Failed to decode response body: %v", err)
    }

    // Verify the number of tasks returned
    if len(tasks) != 1 {
        t.Errorf("Expected 1 task, got %d", len(tasks))
    }

    // Verify the properties of the task
    if tasks[0].Title != "New Task" {
        t.Errorf("Expected task title to be 'New Task', got %s", tasks[0].Title)
    }
    if tasks[0].Done != false {
        t.Errorf("Expected task done status to be false, got %t", tasks[0].Done)
    }
}
```

Our E2E test ensures that the entire application stack, including HTTP request handling and database interactions, works correctly together. It simulates real-world interactions with the API and verifies the behavior of the system as a whole. Let's dive into each part of test:

1. **Set up a Mock Database**: The test begins by setting up a mock database using the `setupMockDB()` function. This function returns a mock database instance that the test will use.
    
2. **Create a New HTTP Server**: Next, the test creates a new HTTP server using `httptest.NewServer()`. It passes the main application's handler function, `getTasksHandler(db)`, which handles requests to the `/tasks` endpoint. The server is deferred to close at the end of the test.
    
3. **Test Creating a New Task**:
    
    * It constructs an HTTP POST request to create a new task. The request body contains JSON data specifying the task's title.
        
    * The request is sent to the server using [`http.DefaultClient.Do`](http://http.DefaultClient.Do)`()`.
        
    * The test checks if the response status code is `http.StatusOK` (200), indicating that the task was created successfully.
        
4. **Test Fetching All Tasks**:
    
    * It constructs an HTTP GET request to fetch all tasks.
        
    * The request is sent to the server using [`http.DefaultClient.Do`](http://http.DefaultClient.Do)`()`.
        
    * The test checks if the response status code is `http.StatusOK` (200), indicating that the tasks were fetched successfully.
        
5. **Decode Response Body**:
    
    * The test decodes the response body, which contains a JSON array of tasks, into a slice of `Task` objects.
        
6. **Verify the Number of Tasks Returned**: The test verifies that the number of tasks returned is as expected.
    
7. **Verify the Properties of the Task**:
    
    * The test verifies that the properties of the task match the expected values. In this case, it checks that the task's title is "New Task" and its done status is false.
        

## **Creating the Balance in-between**

So, where does that leave us? Is one testing strategy better than the other? Not necessarily. The key is finding the right balance between end-to-end and unit testing, leveraging the strengths of each to create a comprehensive testing strategy. But before finding that sweet spot, let's first see what are key differences between are.

### **What's the difference between Unit Testing and End-to-End Testing?**

1. **Comprehensive Coverage:** While unit testing is essential for verifying the correctness of individual components, E2E testing complements it by providing a holistic view of the application. Striking a balance ensures comprehensive test coverage.
    
2. **Early Detection of Issues:** Unit testing catches issues at the code level, allowing for early detection and resolution. E2E testing, on the other hand, identifies problems in the overall system, helping to catch integration issues and unexpected interactions.
    
3. **Efficient Debugging:** When issues arise, a combination of unit and E2E tests simplifies the debugging process. Developers can quickly pinpoint whether an issue lies within a specific component or if it is a result of interactions between components.
    
4. **Dependencies and isolation:** Unit testing aims to isolate units, mocking or stubbing external dependencies to focus solely on the unit being tested. In contrast, e2e testing considers the integration and interaction of various components, including external services and databases.
    
5. **Test environment and test data:** Unit testing often utilizes lightweight test environments and predefined test data to isolate units. E2e testing requires a more realistic test environment that replicates production conditions, including the use of real databases, external services, and representative test data.
    

For critical paths and user flows, end-to-end testing can provide valuable assurance that everything works as expected. But for testing individual functions and edge cases, unit testing is the way to go. By combining both approaches, you can cover your bases and catch bugs at every level of your application.

# **Conclusion**

In the ever-evolving world of software testing, Testing in Production emerges as a crucial practice for ensuring the robustness of applications. By embracing a balanced approach that incorporates both E2E and unit testing, developers and testers can create resilient software that meets user expectations while minimizing the risk of defects in production. Striking this balance empowers development teams to deliver high-quality software in an agile and user-focused manner.

## FAQ's

### Which tool can help in creating a balance between end-to-end and unit tests?

In market, there are very limited test automation framework that actually works which can help you in achieving a perfect balance between both types of testing seamlessly. One of which is [Keploy](http://keploy.io). It allows you to generated automated tests for backend applications across different language and dependencies. It can be used for both end-to-end testing, where you simulate user interactions with the application, and with unit testing, where you test individual components or units of code.

### **Which approach is better?**

Both approaches boast unique benefits and fulfill distinct roles within the testing process. Ideally, a fusion of both unit testing and end-to-end testing should be utilized to attain thorough software testing and ensure high quality.

Unit and end-to-end testing represent two separate testing methodologies, each with its own purpose. Unit tests concentrate on individual code units, aiding in the early detection of bugs and bolstering code quality.

In contrast, end-to-end tests scrutinize the entire application from the user's viewpoint, confirming proper functionality and alignment with business requirements. Both methodologies should be carefully considered and seamlessly integrated into a holistic testing strategy.