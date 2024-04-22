---
title: "How I simulate a response from a Third Party"
datePublished: Fri Apr 19 2024 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: clvaqndls00110alddplk6yla
slug: how-i-simulate-a-response-from-a-third-party
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1713777096878/121f2435-5c58-487a-b9b8-69a33dc2230e.png

---

Whether you're building a web application, a mobile app, or any other software product, integrating with third-party APIs is almost inevitable. But what happens when you need to test your application's behavior without relying on these external services? That's where the magic of simulation comes in handy.

In this blog, we'll explore how you can simulate responses effectively, even if the actual service isn't available.

## What's the needed to simulate third party app?

So, imagine you're developing an app that relies on various external services, like payment gateways or weather APIs. During development, waiting for these services to respond can slow you down significantly. ***Plus, what if the service is undergoing maintenance or has usage limitations?*** Simulating responses allows you to keep working without being dependent on these external factors.

One approach is by using stubs. Stubs are like placeholders that mimic the behavior of the actual service. They return predefined responses based on the inputs they receive. But creating these stubs manually can be time-consuming and error-prone. That's where tools like Keploy come into play.

![Face You Make Robert Downey Jr Meme - Imgflip](https://i.imgflip.com/1sg6z6.jpg align="left")

## How it worked for me?

Well, the first step in simulating a response is to identify the interactions between your application and the third-party service. These interactions could include making API calls, sending requests, or receiving data.

I used Keploy as getting started was fairly easy, as documentation was pretty straightforward. First thing was, we need to download and fire up [**keploy**](https://github.com/keploy/keploy).

```bash
curl -O https://raw.githubusercontent.com/keploy/keploy/main/keploy.sh && source keploy.sh
```

That should download and kick-start the keploy server. You should see something like this:

```bash

       ▓██▓▄
    ▓▓▓▓██▓█▓▄
     ████████▓▒
          ▀▓▓███▄      ▄▄   ▄               ▌
         ▄▌▌▓▓████▄    ██ ▓█▀  ▄▌▀▄  ▓▓▌▄   ▓█  ▄▌▓▓▌▄ ▌▌   ▓
       ▓█████████▌▓▓   ██▓█▄  ▓█▄▓▓ ▐█▌  ██ ▓█  █▌  ██  █▌ █▓
      ▓▓▓▓▀▀▀▀▓▓▓▓▓▓▌  ██  █▓  ▓▌▄▄ ▐█▓▄▓█▀ █▓█ ▀█▄▄█▀   █▓█
       ▓▌                           ▐█▌                   █▌
        ▓
  
version: 2.1.0-alpha6

Keploy CLI

Usage:
  keploy [command]

Available Commands:
  config      manage keploy configuration file
  example     Example to record and test via keploy
  mock        Record and replay ougoung network traffic for the user application
  record      record the keploy testcases from the API calls
  test        run the recorded testcases and execute assertions
  update      Update Keploy 

Flags:
      --debug     Run in debug mode
  -h, --help      help for keploy
  -v, --version   version for keploy

Guided Commands:
  help        Help about any command

Examples:

  Record:
        keploy record -c "docker run -p 8080:8080 --name <containerName> --network keploy-network <applicationImage>" --containerName "<containerName>" --delay 1 --buildDelay 1m

  Test:
        keploy test --c "docker run -p 8080:8080 --name <containerName> --network keploy-network <applicationImage>" --delay 1 --buildDelay 1m

  Config:
        keploy config --generate -p "/path/to/localdir"


Use "keploy [command] --help" for more information about a command.
```

Now that the Keploy downloaded and on standup, it's time to start creating data stubs/mocks.

The first step in simulating a response is to identify the interactions between my application and the third-party service. These interactions could include making API calls, sending requests, or receiving data. Once we've identified these interactions, Keploy can be used to create stubs that mimic the responses you expect from the actual service.

I will be using my application build in NodeJS and having PostgreSQL as database. You can find the [source code here](https://github.com/Sonichigo/JWT-Node-Keploy). I will be using docker to run my postgres database container.

```bash
docker-compose up -d postgres
```

Let's verify the status of container, with `docker ps` :

```bash
CONTAINER ID   IMAGE           COMMAND                  CREATED       STATUS       PORTS                                       NAMES
8a65d1eb0ac9   postgres:latest   "docker-entrypoint.s…"   2 hours ago   Up 2 hours   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   node-jwt_postgres_1
```

Keploy usually captures all network clients (like HTTP clients or DB drivers), so our applications needs to be wrapped up so it can catch them.

```bash
keploy record -c "node app.js"
```

Here, I have wrapped my command to run application, so for any interactions which are happening to or from my application, keploy will be able to capture and stimulate later.

Let's roll up sleeves and dive into creating mocks for our postgres services. To stimulate our database, we need to first have few interaction between application server and third party app i.e. *PostgreSQL Database in this case.*

```bash
curl --location 'http://localhost:8080/api/auth/signin' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username":"user",
    "email":"user@keploy.io",
    "password":"1234",
    "role": ["admin"]
}'

curl --location 'http://localhost:8080/api/auth/signin' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username":"user",
    "email":"user@keploy.io",
    "password":"1234"
}'

curl --location 'http://localhost:8080/api/users'
```

As soon as we run the above script, we can notice that file name `mocks.yml` geting generated with all the interaction captured happened between our application and database. The `Mocks.yml` would look somthing like this:-

```yaml
### Other Mocks/Stubs
...
version: api.keploy.io/v1beta1
kind: Postgres
name: mock-25
spec:
    metadata:
        type: config
    postgresrequests:
        - header: [Q]
          identifier: ClientRequest
          length: 62
          query:
            string: SELECT "id", "username", "email", "password", "createdAt", "updatedAt" FROM "users" AS "users" WHERE "users"."username" = 'user' LIMIT 1;
          msg_type: 81
          auth_type: 0
    postgresresponses:
        - header: [T, D, C, Z]
          identifier: ServerResponse
          length: 62
          authentication_md5_password:
            salt: [0, 0, 0, 0]
          command_complete:
            - command_tag_type: SELECT 1
          data_row: [{row_values: ["1", user, user@keploy.io, $2a$08$RHFEPjrKaf9Www/wphVbDecsbYAJ5DWbp.IDn.QE.KXOOZbDwfmHW, '2024-04-22 05:46:55.485+00', '2024-04-22 05:46:55.485+00']}]
          ready_for_query:
            txstatus: 73
          row_description: {fields: [{field_name: id, table_oid: 16386, table_attribute_number: 1, data_type_oid: 23, data_type_size: 4, type_modifier: -1, format: 0}, {field_name: username, table_oid: 16386, table_attribute_number: 2, data_type_oid: 1043, data_type_size: -1, type_modifier: 259, format: 0}, {field_name: email, table_oid: 16386, table_attribute_number: 3, data_type_oid: 1043, data_type_size: -1, type_modifier: 259, format: 0}, {field_name: password, table_oid: 16386, table_attribute_number: 4, data_type_oid: 1043, data_type_size: -1, type_modifier: 259, format: 0}, {field_name: createdAt, table_oid: 16386, table_attribute_number: 5, data_type_oid: 1184, data_type_size: 8, type_modifier: -1, format: 0}, {field_name: updatedAt, table_oid: 16386, table_attribute_number: 6, data_type_oid: 1184, data_type_size: 8, type_modifier: -1, format: 0}]}
          msg_type: 90
          auth_type: 0
    reqtimestampmock: 2024-04-22T14:15:02.883000616+05:30
    restimestampmock: 2024-04-22T14:15:02.883048073+05:30
connectionId: "8"
...
## Other Mocks and stubs
```

Voila! Our mocks are created. Above, we can notice that the `mock-25` can be corresponded to the Second API Call in the script that we executed earlier with `Keploy RecordMode` . *But we can't be sure that these mock works yet right?* Now, it's time to run the test that have been generated earlier. So let's stop our container instance

```bash
docker rm -f node-jwt_postgres_1

### Response would be
node-jwt_postgres_1
```

and Run the below command :-

```bash
keploy test -c "node app.js"
```

Similar, to how we were wrapping our applicaition command earlier with Keploy record to capture the network interactions, this time instead of record we will use `testMode` . Keploy will start the application, but this time rather than capturing will sends recorded HTTP test cases, and mocks responses for outgoing calls.

```bash
🐰 Keploy: 2024-04-22T14:33:35+05:30    INFO    starting test for of    {"test case": "test-1", "test set": "test-set-2"}
Executing (default): SELECT "id", "username", "email", "password", "createdAt", "updatedAt" FROM "users" AS "users" WHERE "users"."username" = 'user' LIMIT 1;
Executing (default): SELECT "roles"."id", "roles"."name", "roles"."createdAt", "roles"."updatedAt", "user_roles"."createdAt" AS "user_roles.createdAt", "user_roles"."updatedAt" AS "user_roles.updatedAt", "user_roles"."roleId" AS "user_roles.roleId", "user_roles"."userId" AS "user_roles.userId" FROM "roles" AS "roles" INNER JOIN "user_roles" AS "user_roles" ON "roles"."id" = "user_roles"."roleId" AND "user_roles"."userId" = 1;
Testrun passed for testcase with id: "test-1"

--------------------------------------------------------------------

🐰 Keploy: 2024-04-22T14:33:35+05:30    INFO    result  {"testcase id": "test-1", "testset id": "test-set-2", "passed": "true"}
🐰 Keploy: 2024-04-22T14:33:35+05:30    INFO    starting test for of    {"test case": "test-2", "test set": "test-set-2"}
Executing (default): SELECT "id", "username", "email", "password", "createdAt", "updatedAt" FROM "users" AS "users";
Testrun passed for testcase with id: "test-2"

--------------------------------------------------------------------

🐰 Keploy: 2024-04-22T14:33:35+05:30    INFO    result  {"testcase id": "test-2", "testset id": "test-set-2", "passed": "true"}
🐰 Keploy: 2024-04-22T14:33:35+05:30    INFO    starting test for of    {"test case": "test-3", "test set": "test-set-2"}
Executing (default): SELECT "id", "username", "email", "password", "createdAt", "updatedAt" FROM "users" AS "users";
Testrun passed for testcase with id: "test-3"

--------------------------------------------------------------------

🐰 Keploy: 2024-04-22T14:33:35+05:30    INFO    result  {"testcase id": "test-3", "testset id": "test-set-2", "passed": "true"}

 <=========================================> 
  TESTRUN SUMMARY. For test-set: "test-set-2"
        Total tests: 3
        Total test passed: 3
        Total test failed: 0
 <=========================================> 
```

Magic, isn't it? Keploy automatically serves up the previously recorded stub responses!

You can easily create realistic stubs (service virtualization) for any supported dependency. Whether it's Postgres, MySQL, Redis, HTTP (like Twilio, S3, Google Maps), or more, the process remains same. Just simply follow the same steps to set up stubs for seamless testing and development.