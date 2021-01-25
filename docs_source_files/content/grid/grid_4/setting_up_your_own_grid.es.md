---
title: "Configurando tu propio Grid"
weight: 2
---

{{% notice info %}}
<i class="fas fa-language"></i> Page being translated from 
English to Spanish. Do you speak Spanish? Help us to translate
it by sending us pull requests!
{{% /notice %}}

## Different Modes of Grid setup in Selenium 4:
* Standalone
* Hub and Node
* Distributed
* Docker

## Standalone Mode:
The new Selenium Server Jar contains everything you'd need to run a grid. It is also the easiest mode to spin up a Selenium Grid. By default the server will be listening on http://localhost:4444, and that's the URL you should point your RemoteWebDriver tests. The server will detect the available drivers that it can use from the System PATH

```shell
java -jar selenium-server-4.0.0-alpha-7.jar standalone
```

## Hub and Node Mode:

### Start the Hub:
```shell
java -jar selenium-server-4.0.0-alpha-7.jar hub
```

### Register a Node:

```shell
java -jar selenium-server-4.0.0-alpha-7.jar node --detect-drivers
```

### Query Selenium Grid:

In Selenium 4, we've added GraphQL, a new way to query the necessary data easily and get exactly the same.

```shell
curl -X POST -H "Content-Type: application/json" --data '{ "query": "{grid{uri}}" }' -s http://localhost:4444/graphql | jq .
```
<br><br>

## Distributed Mode:

* Step 1: Firstly, start the Event Bus, it serves as a communication path to other Grid components in subsequent steps.

    ```shell
    java -jar selenium-server-4.0.0-alpha-7.jar  event-bus
    ```

* Step 2: Start the session map, which is responsible for mapping session IDs to the node the session is running on:
        
    ```shell 
        java -jar selenium-server-4.0.0-alpha-7.jar sessions
    ```

* Step 3: Start the new session queuer, it adds the new session request to a local queue. The distributor picks up the request from the queue.
        
    ```shell 
        java -jar selenium-server-4.0.0-alpha-7.jar sessionqueuer
    ```

* Step 4: Start the Distributor. All the nodes are attached as part of Distributor process. It is responsible for assigning a node, when a create session request in invoked.

    ```shell 
        java -jar selenium-server-4.0.0-alpha-7.jar distributor --sessions http://localhost:5556 --sessionqueuer http://localhost:5559 --bind-bus false
    ```

* Step 5: Next step is to start the Router, an address that you'd expose to web

    ```shell 
        java -jar selenium-server-4.0.0-alpha-7.jar router --sessions http://localhost:5556 --distributor http://localhost:5553 --sessionqueuer http://localhost:5559
    ```

* Step 6: Finally, add a Node

    ```shell 
        java -jar selenium-server-4.0.0-alpha-7.jar node --detect-drivers
    ```

## Start Standalone Grid via Docker Images

  You can just start a node by the following command:
      
```shell 
    java -jar selenium-server-4.0.0-alpha-1.jar node -D selenium/standalone-firefox:latest '{"browserName": "firefox"}'
```

  You can start the Selenium server and delegate it to docker for creating new instances:
      
```shell 
     java -jar selenium-server-4.0.0-alpha-7.jar standalone -D selenium/standalone-firefox:latest '{"browserName": "firefox"}' --detect-drivers false
```