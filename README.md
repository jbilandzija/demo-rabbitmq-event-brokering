# rabbitmq-event-brokering demo
* ##### Demo service architecture to demonstrate event messaging between producer and multiple consumers
* ###### Big thanks to https://github.com/thombergs for source code!

### Event Brokering with Spring Boot and RabbitMQ
 
This application implements an Event mechanism usable in Microservice environments. 

### Scenario

* a publisher publishes events with one of these topics
  * customer.created
  * customer.edited
  * customer.deleted
  * order.created
  * order.edited
  * order.deleted
  * invoice.created
  * invoice.edited
  * invoice.deleted
* a subscriber subscribes to events of a specific topic from 
  a queue specific to its (micro-)service 
  
![Event scenario](eventing_amqp.png)

### Usage

1. [Install RabbitMQ](https://www.rabbitmq.com/download.html) (if it's installed on another
host than localhost then you need to change the connection properties in `application.properties`) (You can start RabbitMq as Docker container)

1. start a couple instances of the demo application in `subscriber` mode, for example:
   * `./gradlew bootrun -Dspring.profiles.active=subscriber -Dsubscriber.queue=service1Queue -Dsubscriber.routingKey=customer.*`
   * `./gradlew bootrun -Dspring.profiles.active=subscriber -Dsubscriber.queue=service2Queue -Dsubscriber.routingKey=*.created`
   * `./gradlew bootrun -Dspring.profiles.active=subscriber -Dsubscriber.queue=service3Queue -Dsubscriber.routingKey=*.*`
1. start a single instance of the demo application in `publisher` mode:
   * `./gradlew bootrun -Dspring.profiles.active=publisher`
1. check the log output of the publisher and the subscribers to see which events are produced and which events
   are consumed by each subscriber
1. You can open RabbitMq Admin page: `http://localhost:15672/`
