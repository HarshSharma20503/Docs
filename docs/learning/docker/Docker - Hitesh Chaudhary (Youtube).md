# Docker by Hitesh(Youtube)

## What is Docker and its usage?

Docker is an open platform for developing, shipping, and running applications. Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security lets you run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, so you don't need to rely on what's installed on the host. You can share containers while you work, and be sure that everyone you share with gets the same container that works in the same way.

## What can I use Docker for?

- [Fast, consistent delivery of your applications](https://docs.docker.com/get-started/docker-overview/#fast-consistent-delivery-of-your-applications)
- [Responsive deployment and scaling](https://docs.docker.com/get-started/docker-overview/#responsive-deployment-and-scaling)
- [Running more workloads on the same hardware](https://docs.docker.com/get-started/docker-overview/#running-more-workloads-on-the-same-hardware)

## Docker Architecture

![Architecture](./architecture.png)

Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface. Another Docker client is Docker Compose, that lets you work with applications consisting of a set of containers.

## What is image and container

Think of image as like software and your PC or laptop is the container that runs that software. Similarly you build containers to run that image and that container runs on the docker engine. Virtual Machine (VM) runs whole of the operating system over the OS but containers actually contains the software and bare minimum of whatever configuration is required of OS.

## Some useful Commands

### Pulling an image

``docker pull <image-name>`

### View all downloaded images

`docker image ls`

### Running a docker image

`docker run --name <container-name> <image-name>`
some other arguments that you can pass

1. `docker run --name <container-name> -d <image-name>` : this runs the container in detached mode, meaning your terminal won't be used to display output and the container will run in background.
2. `docker run <image-name>:<version` to run a specific version of the image
3. `docker run --network <network-name>` to run a container on specific network

### Running a existing container

`docker start <container-name/container-tag>`

### Stopping a running container

`docker stop <container-name/container-tag>`
or
`docker container stop <container-name/container-tag>`

### Viewing Containers

1. Running containers: `docker ps` or `docker container ls`
2. All containers: `docker ps -a`

### Remove all stoped Containers

`docker container prune`

### See Containers log

`docker logs <container-name/container-id>`

### Write docker commands in multiple line in terminal

```bash
docker run --name test \
-p 8081:8081 \
-e ...
```

## Docker Compose

```yml
services:
    <container-name-1>:
        image: <image-name-1>
        ports:
            - "<host-port>:<container-port>"
        environment:
            - <environment-key>=<environment-value>
        volumes:
            - mymongo-data:/data/db
    <container-name-2>:
        image: <image-name-2>
        restart: always       %% makes sure to restart container if it exits %%
        ports:
            - "<host-port>:<container-port>"
volumes:
    mymongo-data:
        driver: local
```

**Note:** All the containers in a docker-compose.yml file share the same network

### Run docker-compose

`docker-compose -f docker-compose.yml up`

### Close all the containers in docker-compose

`docker-compose -f docker-compose.yml down`

## Dockerfile (Creating an Image)

1. Create the dockerfile in the root dir.
2. Given a example of a flask image

    ```Dockerfile
    FROM python:3-alpine3.15

    WORKDIR /app

    COPY . /app

    RUN pip install -r requirements.txt

    EXPOSE 3000

    CMD python ./index.py
    ```

### Build docker image using dockerfile

`docker build -t <username>/<image-name>:<version> <Dockerfile-path>`

### Run the image built using dockerfile

`docker container run -d -p 3000:3000 <username>/<image-name>:<version>`

### Push image to docker hub

`docker push <username>/<image-name>:<version>`
