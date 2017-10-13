# Useful notes about Docker

* [Docker Products](#docker-products)
   * [Docker Engine](#docker-engine)
   * [Docker Compose](#docker-compose)
   * [Docker Swarm](#docker-swarm)
   * [Docker Machine](#docker-machine)
   * [Docker CLoud](#docker-cloud)
* [Check your images and containers](#check-your-images-and-containers)
   * [List all your images](#list-all-your-images)
   * [List all your containers](#list-all-your-containers)
* [Clean images and containers](#clean-images-and-containers)
   * [Delete an image](#delete-an-image)
   * [Delete a container](#delete-a-container)
   * [Nuclear Clean :)](#nuclear-clean-)
      * [Clean all containers](#clean-all-containers)
      * [Clean all images](#clean-all-images)
* [Passing environment variables, ports and IP addresses](#passing-environment-variables-ports-and-ip-addresses)
   * [Example running a webapp](#example-running-a-webapp)
   * [On which port is your webapp running?](#on-which-port-is-your-webapp-running)
   * [On which port is your webapp running?](#on-which-port-is-your-webapp-running-1)
* [Clean the volumes](#clean-the-volumes)
   * [List dangling volumes:](#list-dangling-volumes)
   * [List all volumes:](#list-all-volumes)
   * [Remove data within the volumes](#remove-data-within-the-volumes)
* [Docker Compose](#docker-compose-1)
   * [Useful commands for Docker Compose](#useful-commands-for-docker-compose)
* [Tag and Push your image to Dockerhub](#tag-and-push-your-image-to-dockerhub)
* [Links](#links)


## Docker Products

The Docker platform is comprised of a family of [tools and products](https://docs.docker.com/manuals/).

List of [guides](https://docs.docker.com/):

* [Docker for Mac](https://docs.docker.com/docker-for-mac/)
* [Docker for Windows](https://docs.docker.com/docker-for-windows/)
* [Docker for Linux](https://docs.docker.com/engine/installation/linux/ubuntu/)
* [Docker CLoud](https://docs.docker.com/docker-cloud/)
* [Docker for AWS](https://docs.docker.com/docker-for-aws/)
* [Docker for Azure](https://docs.docker.com/docker-for-azure/)

Tools:

* [Docker Tools](https://docs.docker.com/compose/overview/)
* [Docker Machine](https://docs.docker.com/machine/overview/)



### Docker Engine

When people say “Docker” they typically mean Docker Engine, the client-server application made up of the Docker daemon, a REST API that specifies interfaces for interacting with the daemon, and a command line interface (CLI) client that talks to the daemon (through the REST API wrapper). Docker Engine accepts docker commands from the CLI, such as `docker run <image>`, `docker ps` to list running containers, `docker images` to list images, and so on.

Docker Engine runs natively on Linux systems. If you have a Linux box as your primary system, and want to run docker commands, all you need to do is download and install Docker Engine. However, if you want an efficient way to provision multiple Docker hosts on a network, in the cloud or even locally, you need Docker Machine.


### Docker Compose

* [Overview Docker Compose](https://docs.docker.com/compose/overview/)

### Docker Swarm

Functionality folded directly into native Docker, no longer a standalone tool.

* [Overview Docker Swarm](https://docs.docker.com/engine/swarm/)

### Docker Machine

* [Docker Engine & Docker Machine](https://docs.docker.com/machine/overview/)
* [Installation](https://docs.docker.com/machine/install-machine/)

### Docker CLoud

Manages multi-container applications and host resources running on a cloud provider (such as Amazon Web Services or Azure).

* [About Docker Cloud](https://docs.docker.com/docker-cloud/)






## Check your images and containers


### List all your images

```shell
docker images
```


### List all your containers

```shell
docker ps -a
```

## Clean images and containers


### Delete an image

```shell
docker rmi <*your image id*>
```

### Delete a container

```shell
docker rm <*your container id*>
```

### Nuclear Clean :)

> If you get an error like **"no space left on device"** and if you don't care about doing a *nuclear clean*, you can:

#### Clean all containers

```shell
docker-compose stop
docker ps -a | sed '1 d' | awk '{print $1}' | xargs -L1 docker rm
```

#### Clean all images 

> Again, careful when using such commands, do not use it in production. The great thing is that if you have your Dockerfile(s), it's easy to rebuild and or docker pull.

```shell
docker-compose stop
docker images -a | sed '1 d' | awk '{print $3}' | xargs -L1 docker rmi -f
```




## Passing environment variables, ports and IP addresses

### Example running a webapp 

```shell
$ docker run --name static-site -e AUTHOR="Your Name" -d -P dockersamples/static-site
```

You can also run a second webserver at the same time, specifying a custom host port mapping to the container's webserver.

```shell
$ docker run --name static-site-2 -e AUTHOR="Your Name" -d -p 8888:80 dockersamples/static-site
```

In the above command:

    `-d` will create a container with the process detached from our terminal
    `-P` will publish all the exposed container ports to random ports on the Docker host
    `-e` is how you pass environment variables to the container
    `--name` allows you to specify a container name
    `AUTHOR` is the environment variable name and Your Name is the value that you can pass



You can stop and remove the containers by their name

```shell
$ docker stop static-site
$ docker stop static-site-2
$ docker rm static-site
$ docker rm static-site-2
```


### On which port is your webapp running?

```shell
docker port static-site
```

### On which port is your webapp running?

```shell
docker port static-site
```


## Clean the volumes

> Warning be very careful with this if you have some data you want to keep.

Additional commands:

### List dangling volumes:

```shell
docker volume ls -qf dangling=true
```

### List all volumes:

```shell
docker volume ls
```

### Remove data within the volumes

```shell
docker volume rm $(docker volume ls -qf dangling=true)
```
## Docker Compose

Getting Started guide: [https://docs.docker.com/compose/gettingstarted/](https://docs.docker.com/compose/gettingstarted/)

### Useful commands for Docker Compose

```shell
docker-compose up   # to start your services
docker-compose -d up   # to start your services in the background (in **detached** mode)
docker-compose stop   # Stop all of your services
docker-compose down --volumes   # Bring everything down (i.e. including the data volumes used by your services)
```
## Tag and Push your image to Dockerhub

* List your docker images to find the ID of the image you want to push

```shell
docker images
```
* Tag your image using this ID, and check it has been tagged properly

```shell
docker tag 71df3012md... patrickmerlot/docker_demo:latest
docker images
```

* Create an account on Dockerhub if not done already, then login

```shell
docker login
```

* Push your image to DockerHub

```shell
docker push patrickmerlot/docker_demo
```

and your done!
Check if it appears online: [https://hub.docker.com/r/patrickmerlot/docker_demo/](https://hub.docker.com/r/patrickmerlot/docker_demo/)




## Links

* [Docker samples](https://docs.docker.com/samples/)
  * [Create a Docker image for a Python Flask app](https://github.com/docker/labs/blob/master/beginner/chapters/webapps.md#23-create-your-first-image)
  * [Deploying to a swarm](https://github.com/docker/labs/blob/master/beginner/chapters/votingapp.md#30-deploying-an-app-to-a-swarm): Docker Swarm allows you to run your containers on more than one machine
* Docker Compose getting-started guide: [https://docs.docker.com/compose/gettingstarted/](https://docs.docker.com/compose/gettingstarted/)
