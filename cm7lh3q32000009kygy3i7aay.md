---
title: "Automate Testing on BitBucket for Golang CRUD App with Docker"
seoTitle: "Automate Testing on Bitbucket for Golang CRUD App with Docker"
seoDescription: "Learn how to set up automated testing on Bitbucket for a Golang CRUD app using Docker, Docker Compose, and CI/CD for efficient DevOps workflows."
datePublished: Wed Feb 26 2025 05:26:43 GMT+0000 (Coordinated Universal Time)
cuid: cm7lh3q32000009kygy3i7aay
slug: automate-testing-on-bitbucket-for-golang-crud-app-with-docker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1740547568469/7e3da485-6c7e-4960-92a8-9236f4a964a1.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1740547593697/7901c77f-7455-4df8-a775-82cf035c309b.png

---

Testing is a crucial aspect of software development, ensuring reliability, functionality, and performance before deployment. In this blog, we will explore how to test a **Golang CRUD application** on **Bitbucket** using **Docker** for database and dependencies. We will approach it from both a **developer's** and a **DevOps** perspective, covering unit tests, integration tests, CI/CD automation, and best practices.

## **Overview of the Application**

We will use a **Golang CRUD application** that utilizes **GraphQL**, **PostgreSQL**, and the **go-chi router**. The app has two main entities:

* `Author`
    
* `Post`
    

The application supports operations like creating, reading, updating, and deleting (`CRUD`) authors and posts.

**Key Technologies Used:**

* **Golang**: The programming language.
    
* **PostgreSQL**: The [database running via docker.](https://keploy.io/blog/technology/secure-your-database-communications-with-ssl-in-docker-containers-learn-to-set-up-ssl-for-mongodb-and-postgresql-efficiently)
    
* **GraphQL**: The API query language.
    
* **Docker & Docker Compose**: For containerizing the application and dependencies.
    
* **Bitbucket Pipelines**: For automated testing and deployment.
    

## **Step 1: Writing Unit Tests**

Unit tests ensure that individual components function correctly. We will use **Go’s testing package (**`testing`) to write unit tests for our CRUD operations alongside of [Keploy UTG](https://keploy.io/unit-test-generator).

### **1.1 Setting Up Testing in Go**

#### **1.2 Sample Unit Test for Panic and Error**

```go
package main

import (
	"bytes"
	"database/sql"
	"encoding/json"
	"fmt"
	"net/http"
	"net/http/httptest"
	"testing"
	"time"

	"github.com/go-chi/chi"
	"github.com/graphql-go/graphql"
	"github.com/graphql-go/handler"
	_ "github.com/lib/pq"
)

// Test generated using Keploy
func TestCheckErr(t *testing.T) {
	defer func() {
		if r := recover(); r == nil {
			t.Errorf("The code did not panic")
		}
	}()
	checkErr(fmt.Errorf("test error"))
}

// Test generated using Keploy
func TestMainFunction(t *testing.T) {
	go func() {
		defer func() {
			if r := recover(); r != nil {
				t.Errorf("The code panicked: %v", r)
			}
		}()
		main()
	}()
	time.Sleep(3 * time.Second)
}
```

#### **Create Author Test**

```go
// Test generated using Keploy
func TestCreateAuthor(t *testing.T) {
	dbInfo := "host=localhost port=5432 user=postgres password=password dbname=postgres sslmode=disable"
	db, err := sql.Open("postgres", dbInfo)
	if err != nil {
		t.Fatalf("Failed to connect to database: %v", err)
	}
	defer db.Close()

	setupSQL := `
		CREATE TABLE IF NOT EXISTS authors (
			id SERIAL PRIMARY KEY,
			name TEXT NOT NULL,
			email TEXT NOT NULL,
			created_at TIMESTAMP NOT NULL
		);
	`
	_, err = db.Exec(setupSQL)
	if err != nil {
		t.Fatalf("Failed to create authors table: %v", err)
	}

	defer func() {
		_, err := db.Exec("DROP TABLE IF EXISTS authors;")
		if err != nil {
			t.Logf("Failed to drop authors table: %v", err)
		}
	}()

	authorType := graphql.NewObject(graphql.ObjectConfig{
		Name:        "Author",
		Description: "An author",
		Fields: graphql.Fields{
			"id": &graphql.Field{
				Type:        graphql.NewNonNull(graphql.Int),
				Description: "The identifier of the author.",
				Resolve: func(p graphql.ResolveParams) (interface{}, error) {
					if author, ok := p.Source.(*Author); ok {
						return author.ID, nil
					}
					return nil, nil
				},
			},
			"name": &graphql.Field{
				Type:        graphql.NewNonNull(graphql.String),
				Description: "The name of the author.",
				Resolve: func(p graphql.ResolveParams) (interface{}, error) {
					if author, ok := p.Source.(*Author); ok {
						return author.Name, nil
					}
					return nil, nil
				},
			},
			"email": &graphql.Field{
				Type:        graphql.NewNonNull(graphql.String),
				Description: "The email address of the author.",
				Resolve: func(p graphql.ResolveParams) (interface{}, error) {
					if author, ok := p.Source.(*Author); ok {
						return author.Email, nil
					}
					return nil, nil
				},
			},
			"created_at": &graphql.Field{
				Type:        graphql.NewNonNull(graphql.String),
				Description: "The created_at date of the author.",
				Resolve: func(p graphql.ResolveParams) (interface{}, error) {
					if author, ok := p.Source.(*Author); ok {
						return author.CreatedAt, nil
					}
					return nil, nil
				},
			},
		},
	})

	rootQuery := graphql.NewObject(graphql.ObjectConfig{
		Name: "RootQuery",
		Fields: graphql.Fields{
			"placeholder": &graphql.Field{
				Type: graphql.String,
				Resolve: func(p graphql.ResolveParams) (interface{}, error) {
					return "placeholder", nil
				},
			},
		},
	})

	rootMutation := graphql.NewObject(graphql.ObjectConfig{
		Name: "RootMutation",
		Fields: graphql.Fields{
			"createAuthor": &graphql.Field{
				Type:        authorType,
				Description: "Create new author",
				Args: graphql.FieldConfigArgument{
					"name": &graphql.ArgumentConfig{
						Type: graphql.NewNonNull(graphql.String),
					},
					"email": &graphql.ArgumentConfig{
						Type: graphql.NewNonNull(graphql.String),
					},
				},
				Resolve: func(params graphql.ResolveParams) (interface{}, error) {
					name, _ := params.Args["name"].(string)
					email, _ := params.Args["email"].(string)
					createdAt := time.Now().UTC()
					var lastInsertID int
					err := db.QueryRowContext(params.Context, "INSERT INTO authors(name, email, created_at) VALUES($1, $2, $3) returning id;", name, email, createdAt).Scan(&lastInsertID)
					if err != nil {
						return nil, err
					}

					newAuthor := &Author{
						ID:        lastInsertID,
						Name:      name,
						Email:     email,
						CreatedAt: createdAt,
					}
					return newAuthor, nil
				},
			},
		},
	})
	schema, err := graphql.NewSchema(graphql.SchemaConfig{
		Query:    rootQuery,
		Mutation: rootMutation,
	})
	if err != nil {
		t.Fatalf("Failed to create schema: %v", err)
	}

	h := handler.New(&handler.Config{
		Schema: &schema,
		Pretty: true,
	})

	r := chi.NewRouter()
	r.Handle("/graphql", h)
	ts := httptest.NewServer(r)
	defer ts.Close()
	mutation := `
		mutation {
			createAuthor(name: "Test Author", email: "test@example.com") {
				id
				name
				email
			}
		}
	`
	requestBody := map[string]interface{}{
		"query": mutation,
	}
	jsonBody, err := json.Marshal(requestBody)
	if err != nil {
		t.Fatalf("Failed to marshal JSON: %v", err)
	}

	resp, err := http.Post(ts.URL+"/graphql", "application/json", bytes.NewBuffer(jsonBody))
	if err != nil {
		t.Fatalf("Failed to send request: %v", err)
	}
	defer resp.Body.Close()

	if resp.StatusCode != http.StatusOK {
		t.Errorf("Expected status 200, got %d", resp.StatusCode)
	}

	var result map[string]interface{}
	if err := json.NewDecoder(resp.Body).Decode(&result); err != nil {
		t.Fatalf("Failed to decode response: %v", err)
	}

	if errors, exists := result["errors"]; exists {
		t.Fatalf("GraphQL errors: %v", errors)
	}

	data, exists := result["data"].(map[string]interface{})
	if !exists {
		t.Fatalf("No data in response")
	}

	authorData, exists := data["createAuthor"].(map[string]interface{})
	if !exists {
		t.Fatalf("No createAuthor data in response")
	}

	if authorData["name"] != "Test Author" {
		t.Errorf("Expected author name 'Test Author', got %v", authorData["name"])
	}

	if authorData["email"] != "test@example.com" {
		t.Errorf("Expected author email 'test@example.com', got %v", authorData["email"])
	}

	if _, exists := authorData["id"]; !exists {
		t.Errorf("No id returned for created author")
	}

	var count int
	err = db.QueryRow("SELECT COUNT(*) FROM authors WHERE email = $1", "test@example.com").Scan(&count)
	if err != nil {
		t.Fatalf("Failed to query database: %v", err)
	}

	if count != 1 {
		t.Errorf("Expected 1 author in database, found %d", count)
	}
}
```

Similarly, we can write test cases for **reading, updating, and deleting authors and posts**.

## **Step 2: E2E API Testing**

E2E API tests check if different components of the application work together correctly.

#### **2.1 Sample API Test for GraphQL API**

We can create our api testcases with [Keploy](https://keploy.io): -

```bash
 curl -O -L https://keploy.io/install.sh && source install.sh
```

Once Keploy is installed now, we will create the binary of our application:-

```bash
go build -cover
```

Once we have our binary file ready,this command will start the recording of API calls using ebpf:-

```bash
sudo -E keploy record -c "./keploy-gql"
```

Make API Calls using Hoppscotch, Postman or cURL command. Keploy with capture those calls to generate the test-suites containing testcases and data mocks, similar to below :

![Testcase](https://github.com/keploy/samples-go/raw/main/graphql-sql/img/testcases.png?raw=true align="left")

## Run the Testcases

Now let's run the test mode (in the graphql-sql directory):

```bash
sudo -E keploy test -c "./keploy-gql" --delay 10
```

we can notice that our first testcase failed due to database configration :

![Failed Test](https://github.com/keploy/samples-go/raw/main/graphql-sql/img/testfailed.png align="left")

Our final test result will look like:-

[![Testrun](https://github.com/keploy/samples-go/raw/main/graphql-sql/img/testrun.png?raw=true align="left")](https://github.com/keploy/samples-go/blob/main/graphql-sql/img/testrun.png?raw=true)

With couple of API calls, we got upto 12.4% of code coverage. 🥳

## **Step 3: Dockerizing the Application**

We will create a `Dockerfile` and a `docker-compose.yml` file to containerize the application and its dependencies.

### **3.1 Dockerfile**

```dockerfile
FROM golang:1.18

WORKDIR /app
COPY . .

RUN go mod download
RUN go build -o main .

EXPOSE 8080
CMD ["./main"]
```

### **Creating the Docker Compose Configuration**

```yaml
version: '3.8'

services:
  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"

  app:
    build: .
    depends_on:
      - db
    ports:
      - "8080:8080"
    environment:
      DB_HOST: db
      DB_PORT: 5432
      DB_USER: postgres
      DB_PASSWORD: password
      DB_NAME: postgres
```

Run the application using:

```sh
docker-compose up --build
```

## **Step 4: Setting Up** [**Bitbucket Pipelines for Automated Testing**](https://keploy.io/blog/technology/bitbucket-self-hosting-running-ebpfprivileged-programs)

Bitbucket Pipelines automates testing and deployment in CI/CD workflows.

### **4.1 Configure** `bitbucket-pipelines.yml`

Create a `.bitbucket-pipelines.yml` file in the project root.

```yaml
image: golang:1.18

pipelines:
  default:
    - step:
        name: Run Tests
        services:
          - postgres
        caches:
          - go
        script:
          - go test ./...
  branches:
    main:
      - step:
          name: Deploy to Production
          script:
            - echo "Deploying to production..."
```

### **4.2 Adding a PostgreSQL Service in Pipelines**

```yaml
definitions:
  services:
    postgres:
      image: postgres
      environment:
        POSTGRES_USER: postgres
        POSTGRES_PASSWORD: password
        POSTGRES_DB: postgres
```

This ensures that PostgreSQL is available when running tests.

## **Step 5: Running Tests in Bitbucket**

Push the changes to Bitbucket and trigger a pipeline run:

```sh
git add .
git commit -m "Added tests and Bitbucket Pipelines"
git push origin main
```

Bitbucket will automatically run the tests defined in `bitbucket-pipelines.yml`.

## **Conclusion**

Testing in Bitbucket Pipelines enhances development by:

1. **Ensuring Code Quality** – Catch bugs before deployment.
    
2. **Automating Tests** – Run tests automatically in each commit.
    
3. **CI/CD Integration** – Deploy only tested and validated code.
    

By leveraging **unit tests, integration tests, Docker, and Bitbucket Pipelines**, we can efficiently test and deploy our **Golang CRUD application** with confidence.

## **FAQs**

### **1\. What is Bitbucket Pipelines?**

Bitbucket Pipelines is a CI/CD tool that automates testing and deployment.

### **2\. Why use Docker in testing?**

Docker ensures a consistent environment across development, testing, and production.

### **3\. How do I debug a failing test in Bitbucket?**

Use `bitbucket-pipelines.yml` to print logs and enable verbose test output:

```yaml
script:
  - go test -v ./...
```

### **4\. Can I run pipelines locally before pushing to Bitbucket?**

Yes! Use **Bitbucket Pipelines Runner** or run tests locally before committing:

```sh
go test ./...
```

### **5\. How do I integrate coverage reports in Bitbucket?**

Modify the pipeline to generate coverage reports:

```yaml
script:
  - go test -cover ./...
```

This helps track test coverage in CI/CD workflows.