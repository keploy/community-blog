---
title: "Build an HTTP server using BunJs and Prisma"
seoTitle: "API server using BunJs and Prisma"
seoDescription: "with Prisma, interacting with databases becomes more intuitive and less erro"
datePublished: Thu May 09 2024 09:35:40 GMT+0000 (Coordinated Universal Time)
cuid: clvz1zagm000308l44asreyxc
slug: build-an-http-server-using-bunjs-and-prisma
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1714976550577/9790f1a8-aac7-42a6-8512-8456792444dd.png
tags: postgresql, prisma, bunjs, prismaorm

---

In this guide, we'll be leveraging two powerful tools: BunJs and Prisma. Together, they provide a robust foundation for constructing modern, scalable, and efficient web servers. But before we dive into the technical details, let's take a moment to understand what BunJs and Prisma bring to the table.***Why use BunJs?***

BunJs is a really lightweight, fast, and highly customizable HTTP framework for Node.js. It helps in simplifying the process of creating web servers. On the other hand, with Prisma, interacting with databases becomes more intuitive and less error-prone, thanks to its powerful features like auto-completion and type checking.

## Setting up our project

First let's install Bun toolkit, with following command: -

```bash
npm install -g bun 
```

Now let's create our project directory: -

```bash
├── controller/
│   ├── comments.controller.ts
│   ├── post.controller.ts
│   └── user.controller.ts
├── prisma/
│   └── schema.prisma
├── services/
│   ├── auth.service.ts
│   ├── comment.service.ts
│   ├── post.service.ts
│   └── user.service.ts
├── docker-compose.yml
├── index.ts
├── package*.json
└── tsconfig.json
```

In `controllers` folder we will define our routes and functions in `services` folder, the `schema.prisma` will contain our model as well as the configurations to run the `Prisma-ORM` . Prisma support different types of SQL based databases, and for this blog we would be using `PostgresQL` with the docker instance.

## Building the services

First we need to install all the packages and dependencies required:-

```bash
bun add -d prisma @types/jsonwebtoken bun-types
bun add pg jsonwebtoken elysia dotenv axios @prisma/client @elysia/cookie
```

We we have prisma installed, we need to create the `schema.prisma` file, where our schema models will be defined.

```bash
bunx init prisma
```

`bunx` is similer to `npx` or `pnpx` the primary purpose of `bunx` is to facilitate the execution of packages that are listed in the `dependencies` or `devDependencies` section of a project's `package.json` file. Instead of manually installing these packages globally or locally, you can use `bunx` to run them directly.

Now create user schema inside `prisma/schema.prisma` file.

```bash
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  password  String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

Next we are going to define our `auth.services.ts` which after authenticating user will grant permission to perform CRUD operations.

```javascript
//auth.service.ts
import jwt from "jsonwebtoken";

export const verifyToken = (token: string) => {
  let payload: any;

  //Verify the JWT token
  jwt.verify(token, process.env.JWT_SECRET as string, (error, decoded) => {
    if (error) {
      throw new Error("Invalid token");
    }

    payload = decoded;
  });

  return payload;
};

export const signUserToken = (data: { id: number; email: string }) => {
  //Sign the JWT token
  const token = jwt.sign(
    {
      id: data.id,
      email: data.email,
    },
    process.env.JWT_SECRET as string,
    { expiresIn: "1d" }
  );

  return token;
};
```

The function `verifyToken()`, which takes a JWT token string as an argument.Inside the function, it calls `jwt.verify` to verify the token. The `jwt.verify` function decodes the token and verifies its signature using the provided secret.

* If there's an error during verification, it throws an error indicating that the token is invalid.
    
* If verification is successful, it assigns the decoded payload to the `payload` variable and returns it.
    

`signUserToken()` function generates a JWT token for a given user data. It uses `jwt.sign` to generate a new JWT token. The payload of the token contains the user's id and email. It uses the JWT secret stored in the environment variable `process.env.JWT_SECRET` for signing the token, the expiry of these tokens are set to 1D by default.

With our authentication mechanism ready in place, we now need to create the `user.service.ts`

```javascript
//user.service.ts
import { prisma } from "../index";
import { signUserToken } from "./auth.service";

export const createNewUser = async (data: {
  name: string;
  email: string;
  password: string;
}) => {
  try {
    const { name, email, password } = data;

    //Hash the password using the Bun package and bcrypt algorithm
    const hashedPassword = await Bun.password.hash(password, {
      algorithm: "bcrypt",
    });

    //Create the user
    const user = await prisma.user.create({
      data: {
        name,
        email,
        password: hashedPassword,
      },
    });

    return user;
  } catch (error) {
    throw error;
  }
};

export const login = async (data: { email: string; password: string }) => {
  try {
    const { email, password } = data;

    //Find the user
    const user = await prisma.user.findUnique({
      where: {
        email,
      },
    });

    if (!user) {
      throw new Error("User not found");
    }

    //Verify the password
    const valid = await Bun.password.verify(password, user.password);

    if (!valid) {
      throw new Error("Invalid credentials");
    }

    // //Sign the JWT token
    const token = signUserToken({
      id: user.id,
      email: user.email,
    });

    return {
      message: "User logged in successfully",
      token,
    };
  } catch (error) {
    throw error;
  }
};
```

Here we have two function:- `createNewUser` and `login` . The `createNewuser` function takes user data including name, email, and password as input, and it hashes the provided password using the `BunJs` package and the bcrypt algorithm for secure storage.

It then attempts to create a new user in the database using the `prisma` ORM's `user.create` method, providing the hashed password along with the name and email. If the user creation is successful, it returns the created user object, otherwise it throws the error.

Whereas, `login` function attempts to find the user with the provided email using the Prisma-ORM's `user.findUnique` method. If the user is not found, it throws an error indicating that the user does not exist. If user exists, then it verifies the provided password against the hashed password stored in the database using the `BunJs` package's `password.verify` method. Upon successful verification, it generates a JWT token using the `signUserToken` function from the `auth.service`, passing the user's id and email. And finally, it returns a success message along with the generated JWT token.

Now with our user Signup and Login is integrated with authentication mechanism and ready, we will move on to create our route file under `controller/user.controller.ts` :-

```javascript
//user.controller.ts
import Elysia from "elysia";
import { createNewUser, login } from "../services/user.service";
```

We are importing the `Elysia` library, for setting up routes and handling HTTP requests. As well the `createNewUser` and `login` functions from the `user.service` module to handle user signup and login functionalities, respectively.

```javascript
app.post("/signup", async (context) => {
  try {
    const userData: any = context.body;

    const newUser = await createNewUser({
      name: userData.name,
      email: userData.email,
      password: userData.password,
    });

    return {
      user: newUser,
    };
  } catch (error: any) {
    return {
      error: error.message,
    };
  }
});
```

`/signup` is a POST route, which expects user data in the request body. Inside the route handler, it extracts user data from the request body. And then calls the `createNewUser` function from the `user.service.ts`, passing the extracted user data.

* If user creation is successful, it returns the created user object.
    
* If an error occurs during the process, it catches the error, extracts the error message, and returns it.
    

After SignUp, next comes the `/login` route :-

```javascript
app.post("/login", async (context) => {
  try {
    const userData: any = context.body;

    const loggedInUser = await login({
      email: userData.email,
      password: userData.password,
    });

    return loggedInUser;
  } catch (error: any) {
    console.log(error);
    return {
      error: error.message,
    };
  }
});
```

We are calling the `login` function from the user service, passing the extracted user data.

* If login is successful, it returns the logged-in user object along with a JWT token.
    
* If an error occurs during the login process, it catches the error, logs it to the console, extracts the error message, and returns it.
    

Now we have everything in place and ready to run, but before that we need `index.ts` and `docker-compose.yml` file. The docker-compose file will content to create and run the postgres instance:-

```yaml
version: '3.9'
services:
    postgres:
        image: postgres:latest
        restart: always
        environment:
          - POSTGRES_DB=postgres
          - POSTGRES_USER=postgres
          - POSTGRES_PASSWORD=password
        ports:
          - '5432:5432'
        volumes:
          - ./sql/init.sql:/docker-entrypoint-initdb.d/init.sql
        networks:
          - keploy-network

networks:
  keploy-network:
    external: true
```

and our `index.ts` file would like:-

```javascript
//index.ts
import Elysia from "elysia";
import { PrismaClient } from "@prisma/client";
import { userController } from "./controllers/user.controller";

//Create instances of prisma and Elysia
const prisma = new PrismaClient();
const app = new Elysia();

//Use controllers as middleware
app.use(userController as any);

//Listen for traffic
app.listen(4040, () => {
  console.log("🦊 Elysia is running at localhost:4040");
});

export { app, prisma };
```

The `index.ts` file will acts as the entry point where the server and database instances are initialized, controllers are registered, and the server starts listening for incoming requests. It orchestrates the setup of the application and exports necessary instances for use in other modules.

Lets start the server

```javascript
bun run dev
```

```javascript
🦊 Elysia is running at localhost:4040
```

## Conclusion

We got to learn how BunJs simplifies server creation, while Prisma streamlines database interactions. In this blog, we got to learn how to create simple BunJs server with authentication in place using JWT tokens, we created user signup and login routes using Elysia.  
  
In next part of this blog, we will test our application using bun-test, cucumber and keploy and learn which is better to use.

## FAQ's

### **Why use JWT tokens for authentication?**

JWT tokens provide a secure way to authenticate users in web applications. They are stateless, meaning no session needs to be stored on the server, and they can be easily shared across different services. Additionally, they can contain custom claims, enabling developers to add additional information to the token payload.

### **What is Bunx, and how does it differ from other package execution tools?**

Bunx is a package execution tool similar to npx or pnpx. Its primary purpose is to facilitate the execution of packages listed in a project's dependencies or devDependencies section without the need for manual installation. Bunx simplifies dependency management by running packages directly from the project's context.

### **What testing tools will be covered in the next part of the blog?**

In the next part of the blog, we will explore testing methodologies using Keploy, bun test, and CucumberJs . These tools offer different approaches to testing backend applications, and we will discuss their strengths and use cases.

### **Can I contribute to BunJs and Prisma?**

Both BunJs and Prisma are open-source projects, and contributions are welcome! You can contribute by submitting bug reports, feature requests, or even code contributions through their respective GitHub repositories. Make sure to follow their contribution guidelines for more information on how to get involved.