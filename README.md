# apache-thrift-on-docker

This project demonstrates the use of Docker containers to provide services using Apache thrift. 
A simple scenario could be :
* A service written in let's say Java running on container 1. 
* A client written in let's say C running on container 2.

Now, the client in container 2 would be able to call any available services (just like using an API) in container 1 without the overhead of REST/SOAP and without having to deal with the language in which the service methods are implemented. 

## Getting Started
* We use `docker-compose.yml` to create 2 containers (Client and Server)
* Client is linked to server in `docker-compose.yml`
* Server exposes port `5000` to serve requests using Apache Thrift
* Server is made aware of exposed ports using the environment variable `SERVICE_PORT`
* Apache thrift C client connects to the server using the `SERVICE_PORT`
* Server responds with response generated on server container

## Prerequisites
* Linux machine
* git
* [Docker](https://docs.docker.com/engine/installation/)
* [docker-compose](https://docs.docker.com/compose/install/#install-compose)

## How to run?
Terminal 1
```sh
$ docker-compose build
...
...
$ docker-compose up
Starting thriftondocker_tserver_1 ...
Starting thriftondocker_tserver_1 ... done
Starting thriftondocker_tclient_1 ...
Starting thriftondocker_tclient_1 ... done
Attaching to thriftondocker_tserver_1, thriftondocker_tclient_1
tserver_1  | eceived: world!, from client
tserver_1  | Server received: world!, from client
...
```

Terminal 2
```sh
$ docker ps
CONTAINER ID        IMAGE                                  COMMAND                  CREATED             STATUS              PORTS                    NAMES
9975e2964bf2        thecuriouscoder/thriftwithdocker:1.0   "/bin/sh -c /bin/bash"   12 minutes ago      Up About a minute                            thriftondocker_tclient_1
bddfe0092fa6        thecuriouscoder/thriftwithdocker:1.0   "bash -c ./server"       12 minutes ago      Up About a minute   0.0.0.0:5000->5000/tcp   thriftondocker_tserver_1

$ docker exec `docker ps -q --filter "name=tclient"` /thrift-book-c_glib/c_glib/simple/client
Hello world!
$ docker exec `docker ps -q --filter "name=tclient"` /thrift-book-c_glib/c_glib/simple/client
Hello world!

```

## Author of Apache Thrift C Examples

* **Simon South** - *Apache thrift C Examples* - [thrift-book-c_glib](https://github.com/simonsouth/thrift-book-c_glib)


## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* [Simon South](https://github.com/simonsouth) for the *easy to understand* Apache Thrift examples in C
* [thecuriouscoder](https://hub.docker.com/r/thecuriouscoder/thriftwithdocker/) for the Docker Image with Apache Thrift
