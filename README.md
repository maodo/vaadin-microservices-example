Example project for working with microservices using Vaadin and Spring boot.

The project was a learning process for creating a micro-services setup from scratch.

1. Create Students microservice using [Spring boot](http://projects.spring.io/spring-boot/) and [Spring Data REST](http://projects.spring.io/spring-data-rest/). Data is simple to test out things, just a Student POJO that is served over REST and stored in a SQL DB. I used [Spring Cloud](http://projects.spring.io/spring-cloud/) as a base since I knew that it was the direction I was going.

2. Create [Eureka](https://github.com/Netflix/eureka/) server with help from Spring Cloud. Same setup as in step 1, just without data. After starting Eureka I configured the URL to Students-service and it registered itself automatically. This means that clients can find the service without knowing the actual URL. It also allows load balancing later.

3. Create [Vaadin](https://vaadin.com) UI with help from [Spring4Vaadin](https://github.com/peholmst/vaadin4spring). From Vaadin perspective the UI is very simple. The UI uses Spring to handle the basic REST call, but uses [Ribbon](https://github.com/Netflix/ribbon) to load balance the calls and Eureka to discover the service. I also needed help from [Spring HATEOAS](http://projects.spring.io/spring-hateoas/) to map the responses, since Spring Data REST automatically uses HATOAS when mapping the Students REST interface.

4. TODO: At this point the Students-service should scale by just adding more nodes, so I can test this by deploying to a cloud service.

5. TODO: Next I want to be able to scale the Vaadin nodes by just adding more nodes, but since Heroku doesn't support sticky sessions I need to add support for shared sessions. Luckily it's very easy with Spring Boot and [Memcached session manager](https://code.google.com/p/memcached-session-manager/)

6. TODO: Add hystrix-dashboard and check that Hystrix streams work

7. TODO Add second service and Zulu for proxying service requests.

8. TODO Add Docker files for all services and deploy to AWS Beanstalk

9. Add asynchronous calls with hystrix

To get started locally:

 * Launch RabbitMQ with port mapped to localhost for easier development
 `docker run --hostname my-rabbit -p 5672:5672 rabbitmq:3`
 
 * Optionally launch RabbitMQ manager
 `docker run -d --hostname my-rabbit --name some-rabbit rabbitmq:3`
 
 * Run each app with (in each app directory). It's probably easiest to start with Eureka and config server.
 `mvn spring-boot:run`
 
Heroku deployment
 
 * Create Heroku apps for each and install toolbelt
 * Enable dyno environment information with
 `heroku labs:enable runtime-dyno-metadata -a <app-name>`
 * Eureka clients need the EUREKA_APP_NAME variable set, config server clients need CONFIG_APP_NAME set.
 * Deploy each app with (in each app directory)
 `mvn clean heroku:deploy -Dheroku.appName=<app-name>`
