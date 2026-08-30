# spring-modular-monolith
An e-commerce application following Modular Monolith architecture using [Spring Modulith](https://spring.io/projects/spring-modulith).
The goal of this application is to demonstrate various features of Spring Modulith with a practical application.

![bookstore-modulith.png](docs/bookstore-modulith.png)

This application follows modular monolith architecture with the following modules:

* **Common:** This module contains the code that is shared by all modules.
* **Catalog:** This module manages the catalog of products and stores data in `catalog` schema.
* **Orders:** This module implements the order management and stores the data in `orders` schema.
* **Inventory:** This module implements the inventory management and stores the data in `inventory` schema.
* **Notifications:** This module handles the events published by other modules and sends notifications to the interested parties.

**Goals:**
* Implement each module as independently as possible.
* Prefer event-driven communication to direct module dependency wherever applicable.
* Store data managed by each module in an isolated manner by using different schema or database.
* Each module should be testable by loading only module-specific components.

**Module communication:**

* **Common** module is an OPEN module that can be used by other modules.
* **Orders** module invokes the **Catalog** module public API to validate the order details
* When an Order is successfully created, the **Orders** module publishes **"OrderCreatedEvent"**
* The **"OrderCreatedEvent"** will also be published to external broker like RabbitMQ. Other applications may consume and process those events.
* **Inventory** module consumes "OrderCreatedEvent" and updates the stock level for the products.
* **Notifications** module consumes "OrderCreatedEvent" and sends an order confirmation email to the customer.

## Prerequisites
* JDK 25
* Docker and Docker Compose
* Your favourite IDE (Recommended: [IntelliJ IDEA](https://www.jetbrains.com/idea/))

Follow the [Installation Guide](docs/installation.md) to install the required tools.

Verify the prerequisites:

```shell
$ java -version
$ docker info
$ docker compose version
$ task --version
```

## Using `task` to perform various tasks:

```shell
$ task test // runs all tests

$ task format // formats java code using spotless

$ task build_image // builds docker image

$ task start // starts app using docker compose
$ task stop
$ task restart
```

* Application URL: http://localhost:8080
* Actuator URL: http://localhost:8080/actuator
* Actuator URL for modulith: http://localhost:8080/actuator/modulith
* RabbitMQ Admin URL: http://localhost:15672 (Credentials: guest/guest)
* Grafana URL: http://localhost:3000

## Deploying on k8s cluster

Set up a Kind cluster following [Installation Guide](docs/installation.md)

```shell
# deploy app to kind cluster 
$ task k8s_deploy

# undeploy app
$ task k8s_undeploy
```

* Application URL: http://localhost:30090
* RabbitMQ Admin URL: http://localhost:30091 (Credentials: guest/guest)
* Grafana URL: http://localhost:30092
