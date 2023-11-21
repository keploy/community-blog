---
title: "Writing a Potions Bank REST API with Spring Boot + MongoDB"
seoTitle: "Building a Potions Bank REST API with Spring Boot + MongoDB | Tutorial"
seoDescription: "This step-by-step tutorial covers the entire development process, from setting up the environment to validating the API. Start building your own PotionsBank"
datePublished: Wed Jun 14 2023 07:31:06 GMT+0000 (Coordinated Universal Time)
cuid: clive7yz400060amrdmi32aek
slug: building-rest-api-with-springboot-mongodb
canonical: https://keploy.io/blog/community/writing-a-potions-bank-rest-api-with-spring-boot-mongodb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1686481985457/77997199-c1d9-41de-a66f-c2d813d329d6.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1686484928949/1ef30fcf-f4b0-439a-b5b7-4ff5bfb18d95.png
tags: mongodb, apis, rest-api, springboot, keploy

---

This blog post provides a complete walkthrough of the process of building a RESTful API for Potions Bank using Spring Boot and MongoDB. We’ll cover the steps involved in setting up the development environment, creating necessary components, and integrating them to create a robust and scalable API. By the end of this tutorial, you will have a solid understanding of how to leverage Spring Boot and MongoDB to build powerful APIs for storing and managing magical potions. For this, using IntelliJ Idea is recommended but you’re free to use any IDE of your convenience that supports Java programs to execute.

**Pre-Requisites -**

1. *Understanding* [*REST APIs*](https://community.keploy.io/a-guide-to-various-api-architectures)*!!*
    
2. *What is* [*Spring Boot*](https://www.ibm.com/topics/java-spring-boot)*?*
    
3. *What is* [*MongoDB*](https://www.mongodb.com/what-is-mongodb)*?*
    

With the required fundamentals in place, we’re good to go!!

### Creating & configuring the Spring Boot application:

![](https://cdn-images-1.medium.com/max/880/1*bduhcOZdFYUCJ4vrbMg72g.png align="left")

Create a new project and select the **Spring Initializr** in the left pane.

![](https://cdn-images-1.medium.com/max/880/1*gr5ytE6jhpEJTgKjz98e8A.png align="left")

Choose a suitable project location for your convenience. And make sure that the details in **Language, Type, JDK, Java, and Packaging** field are the same as shown above.

![](https://cdn-images-1.medium.com/max/880/1*3ytbzroAVdRfXMmPPYpM2Q.png align="left")

Choose any version of Spring Boot except the **(Snapshot)** version of it. In the dependency search bar, search the dependencies **Spring Web** and **Spring Data MongoDB**, select these, and click on **Create**.

This is what the initial project folder looks like-

![](https://cdn-images-1.medium.com/max/880/1*lOFyOPug70v3mexrcI9DnA.png align="left")

After this, your **pom.xml** file should look something like this-

%[https://gist.github.com/Shashwat79802/019b640fa95fc8c02199914d3740a59f] 

Your pom.xml file might be a bit different at the beginning but it should have every dependency mentioned here, or if any dependency mentioned inside of the `<dependency></dependency>` tag is not present, you may add it manually by copying it from the code above and placing it smartly in your file, and **reloading** the project, so the dependencies can be refreshed.

### Configuring MongoDB database:

Sign up at [MongoDB](https://www.mongodb.com/), and then create a free cluster.

![](https://cdn-images-1.medium.com/max/880/1*hcfpL1IxkHeuiiquZWu7sw.png align="left")

After the cluster is created, enter the username and password to create a user, click on **Create User** and then click **Finish and Close** as shown.

![](https://cdn-images-1.medium.com/max/880/1*V3kziwxsnEZWSkk48bTN7A.png align="left")

You’re now ready to create a database and collection inside your database in the MongoDB Cluster.

Next, click on **Browse Collections &gt; Add My Own Data** and name your database and collection as **PotionBank** and **Potions** respectively.

Now, go back to the overview and click on connect.

![](https://cdn-images-1.medium.com/max/880/1*0fTDDvH7kEMDFjrsqR7o2Q.png align="left")

Choose **Drivers** to connect and only copy the string given in point 3 which looks like — `mongodb+srv://<username>:<password>@cluster.flihlkb.mongodb.net/?retryWrites=true&w=majority`

Now, in the Spring Boot project, open the `application.properties` file present inside `src.main.resources` folder and add the line mentioned in the code block below:

%[https://gist.github.com/Shashwat79802/e34359086f22a8f34d38b1fb30cf529d] 

Replace the `CONNECTION_STRING` with the string you copied from MongoDB and make sure to replace the username and password placeholders with the credentials of the MongoDB user created earlier.

Your database is all set and integrated now.

### Developing the API:

The Potions Bank is all about storing magical potions. So let’s define the fields that describe any potion. It’s known as defining the model and in this case, defining the Potions model.

1. Inside the `com.example.potionsbank` package, create a **model** folder, and create a file named `Potion.java` inside it, which looks like this:
    
    %[https://gist.github.com/Shashwat79802/f0d5ca83b5b5eca9e71cdf838fb06b84] 
    
    In line 12 of the `Potion.java` file, replace the value **Collection\_Name -&gt; Potions** assigned to `collection` which is the name of the collection we created in the MongoDB database. Here, we define and associate **id, name, description, bottle,** and **quantity** fields with our **Potion** model with a bunch of constraints such as `@NotBlank`, `@Size(max=200)`, etc. Also, we use getter and setter methods with each field because the users shouldn’t be allowed directly manipulate the data, so the areas are made private and are allowed to be manipulated only via the getter and setter methods.
    
2. Create a folder named **repository** inside of the package `com.example.potionsbank` and create `PotionRepository.java` file in it and paste the code present below:
    
    %[https://gist.github.com/Shashwat79802/1797588ad7bd99eb84737cb1a973bcb0] 
    
    Here, we create an interface `PotionRepository` which reflects the data to and fro the MongoDB database that takes two arguments: a Potion object and a UUID id related to the object.
    
3. Now, create a folder named **exception i**n the same package in which we’ve been creating folders till now. And we create a `ResourceNotFoundException.java` file inside it. The contents of this file are:
    
    %[https://gist.github.com/Shashwat79802/22944671b0bb83cb0ea25a0671d5c9bf] 
    
    `ResourceNotFoundException` is what our server returns whenever the requested data is not found in the database.
    
4. Create `PotionService.java` file inside a new folder named **service** in the same package and paste the code provided below:
    
    %[https://gist.github.com/Shashwat79802/53832883535e44932b7bdf060f33de1f] 
    
    This interface defines a convention to manipulate the Potions model by performing CRUD operations over it. The implementation of this interface is provided in the next step.
    
5. Inside the **service** folder, create the `PotionServiceImpl.java` file. This class implements the `PotionService` interface i.e. the methods provided in it.
    
    %[https://gist.github.com/Shashwat79802/75c45eda568c3dab26b55f0c68257534] 
    
    Through this, we’re able to perform the following operations over our model: Create data, update existing data, get all the data, get specific data by ID, and delete data.
    
6. The last step is to create and define the endpoints of the API which we do by defining a `RestController`. For this, create a file named `PotionController.java` inside the **controller** directory which has to be created in the `com.example.potionsbank` package. The controller looks like this-
    
    %[https://gist.github.com/Shashwat79802/97f800c57508f458b2dcb497ed5e60c1] 
    
    Here, we defined certain annotations like `@GetMapping(“/potions”)`, `@DeleteMapping(“/potions/{id}”)`, `@PutMapping(“/potions/{id}”)`, etc. which act as API endpoints, and the respective operations will be performed when these endpoints are hit. Respective methods are associated with these annotations which in turn call the methods defined inside of `PotionServiceImpl.java` to do CRUD operations over the data.
    

And this completes the Potions Bank API. To run this application, open the `PotionsBankAppliation.java` file which contains the `main()` method that acts as the entry point for the application. Executing it will start running the application on `localhost:8080` which is the default port that the Spring Boot uses to run its applications.

![](https://cdn-images-1.medium.com/max/880/1*SVojWXmeYFy8ShVsl5ZiIg.png align="left")

This shows that the application is up and running without any errors, if you face any abnormality or any exit code, then there may be some error and you should consider revisiting the error showing parts of it.

Since the application is running fine, we must test it by hitting the defined endpoints and try manipulating the data. For this, I’m using the **Postman** desktop client to check to validate whether API is working fine or not.

### Making and validating the API calls:

1. According to the endpoints defined in `PotionController.java`, making the API call as a `GET` request to the root URL i.e. `http://localhost:8080` must return a **Hello World** response with a status code of **200 OK.**
    
    ![](https://cdn-images-1.medium.com/max/880/1*XwVK2YC2wPUBj_RSa6R43A.png align="left")
    
    So, the root endpoint works just fine!!
    
2. Making a `POST` request to the **/potions** endpoint with the data of any magical potion as a JSON object should add the data to the database and return the data with its assigned UUID included.
    
    ![](https://cdn-images-1.medium.com/max/880/1*QQ9-WlLWR6uxzW6haKrk2g.png align="left")
    
    Here, a POST request was made with the data of Flying Potion which was successfully added to the database, and as a response, the same object was returned with a new **id** field added to it.
    
3. Now, to verify whether the data in the previous step is surely added to the database or not, we make a `GET` request to the `/potions/{id}` endpoint, replacing the placeholder id with the UUID received as the response above.
    
    ![](https://cdn-images-1.medium.com/max/880/1*VqhKwnk58pZCI_x4yWVK_A.png align="left")
    
    Therefore, making the `GET` request with the appropriate id in place, the same Potion object was received as a response. If in case, the object data wasn’t added to the database or a wrong UUID was provided with the request, the following response will be received:
    
    ![](https://cdn-images-1.medium.com/max/880/1*PzeuCctQWtVICW36WB29bw.png align="left")
    
    And this response says that `ResourceNotFoundException` was encountered which means the data is either not present or a wrong UUID is provided.
    
4. Now to view all the Potions present in our Potions Bank, make a `GET` request to the `/potions` endpoint:
    
    ![](https://cdn-images-1.medium.com/max/880/1*hf3aVbf0QtaA0xZ2tlKaiQ.png align="left")
    
    This gives the details of all the available potions in the bank.
    
5. Let’s now try to update the data of the Flying Potion we just added. And to do so, we make a `PUT` request to the `/potions/{id}` endpoint, replacing the placeholder id with the appropriate UUID of Flying Potion. And what we update is, we’ve got a new update for the Flying Potion as Flying Potion v2.0 with 4 bottles and a total quantity of 200ml.
    
    ![](https://cdn-images-1.medium.com/max/880/1*EoJi3Jzx_Gv4th4VmxScsQ.png align="left")
    
    So, on making the request, we get the updated response of the object with its UUID having updated values in place. You may verify it by making a `GET` request to the `/potions/{id}` endpoint.
    
6. Finally, to delete the data, make a `DELETE` request to `/potions/{id}` endpoint. Let’s try to delete the Flying Potion v2.0 and if the operation is successful, we get “**OK”** as the response.
    
    ![](https://cdn-images-1.medium.com/max/880/1*YV__UtU0jVCvM3eRdM52Gw.png align="left")
    
    If this operation fails due to any reason, `ResourceNotFoundException` will be encountered again.
    

And in this way, we finally validate the request and response to the Potions Bank REST API.

### Conclusion:

We covered all the steps required to create a RESTful API and integrate it with the MongoDB database. You may add some business logic to try and manipulate this API at your convenience.

Hope this tutorial helps you to understand the fundamentals and steps of building the REST API.

In case of any doubts or issues, please feel free to reach out and connect with me on my socials- [Twitter](https://twitter.com/Shashwat_g27), [LinkedIn](https://www.linkedin.com/in/shashwat--gupta/), or [Email](http://shashwatg79802@gmail.com) 🌻