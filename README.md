## Ama-Food-Delivery

A distributed message-based food ordering system developed with RabbitMQ, SpringBoot, Spring AOP, Spring Data Jpa and PostGreSQL. It allows clients to place orders and track them. It consists of three integral components - order-service, restaurant-service and email-service. The order-service exposes REST endpoints that allow clients to fetch the food menu, place an order and track the order in real-time. Once the client places an order, the order-service persists the order details on a PostGreSQL DB server, publishes it to a RabbitMQ Exchange and returns a confirmation response which includes the order ID to track the order. The Exchange is configured with a fanout pattern which publishes the order to the two queues that have been bound to it - order.process and order.confirmation. While placing it on the queue, the 'status' field of the order holds the value 'pending'. A restaurant-service consumes the order from the order.process queue and an email-service consumes the order from the order.confirmation queue. Once the restaurant-service consumes the order, it modifies the 'state' of the order to 'accepted' in the database. The email-service on the other hand sends an order confirmation to the email address specified in the order. After a pre-defined time period, the restaurant-service modifies the 'state' to 'delivered' in the database indicating that the order has been delivered.


This application uses a message-based architecture to enhance scalability, flexibility and loose coupling among the various services it comprises. RabbitMQ, by default, ensures that all the messages consumed from a queue are distributed in a round-robin fashion. Since the restaurant-service and email-service consume from these queues, they can be independently scaled - though physically, scaling out the restaurant-service would mean opening new restaurants. The services are configured to process only a limited number of orders at a time to mimic actual business since restaurants have limited staff. The remaining orders wait on the queue to be consumed. The restaurant-service sends an 'acknowledge' to RabbitMQ once the order is fully processed - delivered, in our case - to prevent it from being added back onto the queue. If one of the restaurants stop functioning say, due to a power loss, RabbitMQ would be aware that the tcp connection between the restaurant-service and the Exchange has been closed and any order that was being processed is automatically added back onto the queue; ready to be consumed by another restaurant-service. This enhances the fault-tolerance of the system.



## High Level Design 

![image](https://github.com/coding-nomadic/Ama-Food-Delivery/assets/8009104/cdfe86f1-3c3c-4fcc-a208-f17aeb8ddff0)

## Features in this application- 

- Ama food owner can create menus and save it.
- Customer orders food
- Gets notification to restaurants
- Email confirmation on orders

## Tools and Technologies

- Java 11
- Spring Boot
- Spring Web MVC
- Spring Aamqp for rabbit mq
- Rabbit mq server
- Spring Data JPA
- Spring AOP
- Spring Mail
- Apache Maven
- PostgreSQL Database
- Docker
- Junit and Integration Testing with testcontainers



