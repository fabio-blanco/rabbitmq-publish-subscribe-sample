 # RabbitMQ Java Sample: Publish/Subscribe
 
This project is a simple sample implementation of a message based application Publish / Subscribe type
using RabbitMQ as the message broker to send messages to many consumers at once.

The system is constituted by two simple applications, a consumer and a producer. And both 
applications communicate by using a RabbitMQ message queue. Any instances of the consumer will be
listening to a queue bound to the same fanout exchange.

This sample code is based on the [official RabbitMQ Publish/Subscribe tutorial](https://www.rabbitmq.com/tutorials/tutorial-three-java.html).

## How to run the sample

In order to execute the app it is required to have [docker](https://docs.docker.com/engine/install/) 
and [docker compose](https://docs.docker.com/compose/install/) installed. 

Run the app by executing this command on a linux terminal from inside the project directory:
```shell
COMPOSE_DOCKER_CLI_BUILD=1 DOCKER_BUILDKIT=1 docker compose up --build
```

This will start one RabbitMQ container and one application container with two consumer
app instances running in parallel.

Now from another terminal run this in order to open a bash shell inside the application
container:
```shell
docker exec -it rabbit_publish_subscribe_sample /bin/bash
```

Then just call the producer app to send a message:
```shell
java -jar producer.jar Hello Log World
```

You can see the message, printed by the consumer, on the first terminal.

One consumer will be printing the messages to the terminal and the other to a logfile that will be 
generated in the ./logs directory.

The RabbitMQ manager front end can be accessed by opening this address on your browser `http://localhos:15672`.
Use the default user and password to login `guest` and `guest`.

## How to Debug

In order to debug the consumer app for debugging purposes. Run it with the development docker compose:
```shell
COMPOSE_DOCKER_CLI_BUILD=1 DOCKER_BUILDKIT=1 docker compose -f docker-compose.dev.yml up --build
```

Wait until the worker is ready and connect to it with your ide using remote jvm debug connection
on port 5005.

Now from another terminal run this in order to open a bash shell inside the application
container:
```shell
docker exec -it rabbit_publish_subscribe_sample /bin/bash
```

Place a breakpoint inside the `deliverCallback` then just call the producer app to send a message:
```shell
./gradlew -q run -p producer --args="Do log this"
```

# License

Released under [MIT Lisense](https://github.com/fabio-blanco/rabbitmq-publish-subscribe-sample/blob/main/LISENCE).
