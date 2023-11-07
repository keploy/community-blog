---
title: "Building a GO CRUD Rest API from scratch"
datePublished: Tue Jun 20 2023 08:30:42 GMT+0000 (Coordinated Universal Time)
cuid: clj40zqin0983q7nv7ab2ba4w
slug: building-a-crud-application-from-scratch-using-golang
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1687099101486/2378d629-8a51-4fff-b435-b379da1a38e1.png
tags: postgresql, docker, rest-api, docker-compose, keploy

---

Thanks to [**Francesco Ciulla**](https://hashnode.com/@FrancescoCiulla) for making a detailed [video](https://www.youtube.com/watch?v=aLVJY-1dKz8&t=295s) on how to create this API which was really helpful to understand concepts.

I have created a GO CRUD Rest API which keeps account of Mens 100m Race using:

* Mux (Framework to build web servers in Go)
    
* Postgres (relational database)
    
* Docker (for containerization)
    
* Docker Compose
    

All the code is available in the GitHub repository ([go-api](https://github.com/ShivangShandilya/go-api))

---

## GAME PLAN

Here is a schema of the architecture of the application we are going to create:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685547553530/04c34008-9347-43cb-997c-e00f9c0b6830.png align="center")

We will create 5 endpoints for basic CRUD operations:

* Create
    
* Read all
    
* Read one
    
* Update
    
* Delete
    

So, here are the steps we are gonna go through:

1. Create a Go application using Mux as a framework
    
2. Dockerize the Go application by writing a Dockerfile and a docker-compose.yml file to run the application and the database.
    
3. Run the SQL database in a container using Docker Compose, and test it with TablePlus.
    
4. Build the Go App image and run it in a container using Docker Compose, then test it with Postman.
    

#### STEP BY STEP GUIDE is given below

---

## Create a Go application using Mux as a framework

1. Create a new folder for your project
    
2. Initialize a go module using this command
    

```plaintext
go mod init api
```

1. Install the dependencies:
    

```plaintext
go get github.com/gorilla/mux github.com/lib/pq
```

We now need 3 more files to start with our project, One of them is going to be our `main.go` file where our Golang code for API will go and the other too will be our `Dockerfile` and a `Docker Compose` file.

```plaintext
touch main.go Dockerfile docker-compose.yml
```

LET'S OPEN VS CODE AND START WORKING ON OUR API

```plaintext
code .
```

When your VS Code opens it should look like:

![image](https://user-images.githubusercontent.com/101946115/241856896-a0de73c9-49d9-4efb-99bd-4473df329a3d.png align="left")

---

## Working on our main.go file

The main.go file is the main file of the application: it contains all the endpoints and the logic of the app.

Populate the main.go file as follows:

```go
package main

import (
	"database/sql"
	"encoding/json"
	"log"
	"net/http"
	"os"

	"github.com/gorilla/mux"
	_ "github.com/lib/pq"
)

type Men struct {
	Rank    int    `json:"rank"`
	Name    string `json:"name"`
	Country string `json:"country"`
}

func main() {
	//connection to database
	db, err := sql.Open("postgres", os.Getenv("DATABASE_URL"))
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	//create table if it doesn't exist
	_, err = db.Exec("CREATE TABLE IF NOT EXISTS men(rank int, name text, country text);")
	if err != nil {
		log.Fatal(err)
	}

	//create router
	router := mux.NewRouter()
	router.HandleFunc("/men", getMen(db)).Methods("GET")
	router.HandleFunc("/men/{id}", getMenByID(db)).Methods("GET")
	router.HandleFunc("/men", createUser(db)).Methods("POST")
	router.HandleFunc("/men/{id}", updateUser(db)).Methods("PUT")
	router.HandleFunc("/men/{id}", deleteUser(db)).Methods("DELETE")

	//start server
	log.Fatal(http.ListenAndServe(":8000", jsonContentTypeMiddleware(router)))

}

func jsonContentTypeMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("Content-Type", "application/json")
		next.ServeHTTP(w, r)
	})
}

// get all mens
func getMen(db *sql.DB) func(w http.ResponseWriter, r *http.Request) {
	return func(w http.ResponseWriter, r *http.Request) {
		rows, err := db.Query("SELECT rank, name, country FROM men")
		if err != nil {
			log.Fatal(err)
		}
		defer rows.Close()

		var men Men
		for rows.Next() {
			err := rows.Scan(&men.Rank, &men.Name, &men.Country)
			if err != nil {
				log.Fatal(err)
			}
		}

		if err := rows.Err(); err != nil {
			log.Fatal(err)
		}

		json.NewEncoder(w).Encode(men)
	}
}

// get user by id
func getMenByID(db *sql.DB) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		id := mux.Vars(r)["id"]
		row := db.QueryRow("SELECT rank, name, country FROM men WHERE id = $1", id)
		var men Men
		err := row.Scan(&men.Rank, &men.Name, &men.Country)
		if err != nil {
			log.Fatal(err)
		}

		json.NewEncoder(w).Encode(men)
	}
}

// create user
func createUser(db *sql.DB) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		var men Men
		err := json.NewDecoder(r.Body).Decode(&men)
		if err != nil {
			log.Fatal(err)
		}

		_, err = db.Exec("INSERT INTO men (name, country) VALUES ($1, $2)", men.Name, men.Country)
		if err != nil {
			log.Fatal(err)
		}

		json.NewEncoder(w).Encode(men)
	}
}

// update user
func updateUser(db *sql.DB) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		id := mux.Vars(r)["id"]
		var men Men
		err := json.NewDecoder(r.Body).Decode(&men)
		if err != nil {
			log.Fatal(err)
		}

		_, err = db.Exec("UPDATE men SET name = $1, country = $2 WHERE id = $3", men.Name, men.Country, id)
		if err != nil {
			log.Fatal(err)
		}

		json.NewEncoder(w).Encode(men)
	}
}

// delete user
func deleteUser(db *sql.DB) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		id := mux.Vars(r)["id"]
		_, err := db.Exec("DELETE FROM men WHERE id = $1", id)
		if err != nil {
			log.Fatal(err)
		}

		json.NewEncoder(w).Encode(id)
	}
}
```

So there are some of the things we are doing in our `main` function:

* We are connecting to Postgres DB setting an environment variable.
    
* We are creating a table if it doesn't already exist
    
* We use Mux to handle 5 endpoints
    
* We are listening to the server on port 8000
    

---

## **🐳** Dockerizing our GO Application

Let's populate the `Dockerfile` :

```plaintext
# use official Golang image
FROM golang:1.16.3-alpine3.13

# set working directory
WORKDIR /app

# Copy the source code
COPY . . 

# Download and install the dependencies
RUN go get -d -v ./...

# Build the Go app
RUN go build -o api .

#EXPOSE the port
EXPOSE 8000

# Run the executable
CMD ["./api"]
```

Explanation:

`FROM` sets the base image to use. In this case, we are using the golang:1.16.3-alpine3.13 image, a lightweight version

`WORKDIR` sets the working directory inside the image

`COPY . .` copies all the files in the current directory to the working directory

`RUN go get -d -v ./...` Is a command to install the dependencies before building the image

`RUN go build -o api .` build the Go app inside the Image filesystem

`EXPOSE 8000` exposes the port 8000

`CMD ["./api"]` sets the command to run when the container starts

---

## **🐳** Setting up Docker Compose

The term "Docker compose" might be a bit confusing because it's referred to both to a file and to a set of CLI commands. Here we will use the term to refer to the file.

Populate the `docker-compose.yml` file:

```plaintext
version: '3.9'

services:
  go-app:
    container_name: go-app
    image: shivangshandilya/go-app:1.0.0
    build: .
    environment:
      DATABASE_URL: "host=go_db user=postgres password=postgres dbname=postgres sslmode=disable"
    ports:
      - "8000:8000"
    depends_on:
      - go_db
  go_db:
    container_name: go_db
    image: postgres:12
    environment:
      POSTGRES_PASSWORD: postgres
      POSTGRES_USER: postgres
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:  
  pgdata: {}
```

**Explanation**: we just defined 2 services, `go-app` and `go_db`

`go-app` is the Go application we just Dockerized writing the Dockerfile

`go_db` is a Postgres container, to store the data. We will use the official Postgres image

`version` is the version of the docker-compose file. We are using version 3.9

`services` is the list of services (containers) we want to run. In this case, we have 2 services: "go-app" and "go\_db"

`container_name` is the name of the container. Containers find each other by their name, so it's important to have a name for the containers we want to communicate with.

`image` is the name of the image we want to use. I recommend replacing "dockerhub-" with YOUR Dockerhub account.

`build` is the path to the Dockerfile. In this case, it's the current directory, so we are using `.`

`ports` are the list of ports we want to expose. In this case, we are exposing the port 8000 of the go-app container, and the port 5432 of the go\_db container. The format is "host\_port:container\_port"

`depends_on` is the list of services we want to start before this one. In this case, we want to start the Postgres container before the app container.

`environment` is to define the environment variables. for the go-app, we will have a database url to configure the configuration. For the go\_db container, we will have the environment variables we have to define when we want to use the Postgres container (we can't change the keys here, because we are using the Postgres image, defined by the Postgres team).

`volumes` in the go\_db define a named volume we will use for persistence. Containers are ephemeral by definition, so we need this additional feature to make our data persist when the container will be removed (a container is just a process).

`volumes` at the end of the file is the list of volumes we want to create. In this case, we are creating a volume called `pgdata`. The format is `volume_name: {}`

---

## Running our Postgres container and testing it with TablePlus

To run the Postgres container, type:

```plaintext
docker compose up -d go_db
```

The `-d` flag is to run the container in detached mode, so it will run in the background.

Now that the Postgres container is up and running, we can check the db connection via TablePlus.

Use the following configuration:

Host: [`localhost`](http://localhost)

Port: `5432`

User: `postgres`

Password: `postgres`

Database: `postgres`

![Tableplus interface](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/s8zt4zbr169a8xxerx92.png align="left")

Then hit "Test" (at the bottom-right).

If you get the message "connection is OK" you are good to go.

![TAbleplus, OK connection](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/96boisx1566xyj1bqwzn.png align="left")

You can also click "Connect" and you will see an empty database. This is correct.

![Tableplus empty but connected db](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/em8sr7gb57fj5w12cxhp.png align="left")

---

## Building the GO Application

Now, let's build and run the GO application.

Let's go back to the folder where the `docker-compose.yml` is located and type:

```plaintext
docker compose build
```

This should BUILD the go-app image, with the name defined in the "image" value. In my case, it's shivangshandilya/go-app:1.0.0 because that's my Dockerhub username. You should replace "shivangshandilya" with your Dockerhub username.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685595241640/8f8175ac-a0a9-4a25-a2b7-4916abe9d42e.png align="center")

---

## Running the go-app service

We are almost done, but one last step is to run a container based on the image we just built.

To do that, we can just type:

```plaintext
docker compose up go-app
```

In this case we don't use the -d flag, because we want to see the logs in the terminal.

We should see something like this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685595355507/b24609bb-d0fd-4110-9ff1-1ff5f59f4b7d.png align="center")

---

## Testing the application

Let's test our application. First of all, let's see if the application is responding. To do this, make a GET request to [`localhost:8000/`](http://localhost:8000/users)`men`

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685595459005/c7267f02-eaaa-4daf-9fec-8cbc5132f888.png align="center")

---

## **📝 Creating an entry**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685595534990/168a50af-c264-4039-931a-73617193bf92.png align="center")

If you create some more entries and then take a look at TablePlus it will look kinda like this:

![image](https://user-images.githubusercontent.com/101946115/241857223-2c5a1454-794e-4ab6-860b-138ac20e7b54.png align="left")

---

## 🏁 Conclusion

So if you have been following along your CRUD rest API in Go, using Mux, Postgres, Docker and Docker compose is created.

I hope you had fun creating this API as much as I had. That is all for today but you can follow me on [LinkedIn](https://www.linkedin.com/in/shivang-shandilya/) and [Twitter](https://twitter.com/shivv_twt) to be updated on any blogs I might push out.  
Would love to hear your views down below!!