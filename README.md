# apache-thrift-on-docker

Apache Thrift example in C using [simonsouth/thrift-book-c_glib](https://github.com/simonsouth/thrift-book-c_glib)

* We use docker-compose to create 2 containers (Client and Server)
* Client is linked to server in `docker-compose.yml`
* Server exposes port `5000` to serve requests using Apache Thrift
* Server is made aware of exposed ports using the environment variable `SERVICE_PORT`
* Apache thrift C client connects to the server using the `SERVICE_PORT`
* Server responds with response generated on server container

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

$ docker exec thriftondocker_tclient_1 /thrift-book-c_glib/c_glib/simple/client
Hello world!
$ docker exec thriftondocker_tclient_1 /thrift-book-c_glib/c_glib/simple/client
Hello world!

```
