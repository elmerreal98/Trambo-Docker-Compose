# Trambo-Docker-Compose
Marzo 4, 2020. Ejercicio de docker compose

## Introduction
To do this exercise, i used the following technologies/tools:
- Flask is a micro web framework written in Python. It is classified as a microframework because it does not require particular tools or libraries. It has no database abstraction layer, form validation, or any other components where pre-existing third-party libraries provide common functions.

- Nginx is a web server which can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache.

- Redis is an in-memory data structure project implementing a distributed, in-memory key-value database with optional durability. Redis supports different kinds of abstract data structures, such as strings, lists, maps, sets, sorted sets, HyperLogLogs, bitmaps, streams, and spatial indexes.

- Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

- Docker Compose  is a tool for defining and running multi-container Docker applications.

A brief explanation of what the exercise consist is:

- There have to be a redis container, which will store data sended by the flask container.
- There have to be a nginx container, which will serve static files to the user.
- There have to be another nginx container, which will work as a proxy in order to redirect the trafic to the flask container or to the nginx static files container depending on a specific prefix. 


## Folder Hierarchy

```
./
│   README.md
│   docker-compose.yml    
│
└───flaskserver
│   |   app.py
│   |   Dockerfile
|   |   requirements.txt
|   
└───nginx_proxy
│   │   Dockerfile
│   │
│   └───proxyfiles
│       │   reverse-proxy.conf
│   
└───nginx_static
    │   Dockerfile
    |
    └───page
        │   index.html 
```

### [docker-compose.yml](/docker-compose.yml)
This file allows run and comunicate multiple containers easly.

The services declared were:

- flaskserver

This service works on the port number 5000

For this service were needed build a specific image based on the Dockerfile located in the folder [/flaskserver](/flaskserver)

Also were declared a link with redisdb service in order to communicate the redis db with this flask application.

```
flaskserver:
    build: flaskserver
    ports:
      - 5000:5000
    depends_on:
      - redisdb
    environment:
      - ENV_HOST=database #Debe ser el mismo nombre que el alias de redisdb
      - ENV_PORT=6379
      - ENV_DB=0
    links:
      - "redisdb:database"
```

- redisdb

This service works on the port number 6379

This service uses a redi image from the docker hub.


```
redisdb:
    image: 'bitnami/redis:latest'
    ports:
      - 6379:6379
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
```


- staticserver

This service works on the port number 8080

For this service were needed build a specific image based on the Dockerfile located in the folder [/nginx_static](/nginx_static)


```
staticserver:
    build: nginx_static
    ports:
      - 8080:80
```

- proxy

This service works on the port number 80

For this service were needed build a specific image based on the Dockerfile located in the folder [/nginx_proxy](/nginx_proxy)

Also were declared a link with the flask service and the nginx static files server in order to be able to redirect the traffic.

```
  proxy:
    build: nginx_proxy
    ports:
      - 80:80
    environment:
      - ENV_PORT=80
      - ENV_HOST=54.202.8.127
      - ENV_PYTHON_HOST=pythonapp
      - ENV_PYTHON_PORT=5000
    links:
      - "flaskserver:pythonapp"
      - "staticserver:nginxstatic"
```
