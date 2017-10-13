# Useful notes about Docker


* [Check your images and containers](#check-your-images-and-containers)
   * [List all your images](#list-all-your-images)
   * [List all your containers](#list-all-your-containers)
* [Clean images and containers](#clean-images-and-containers)
   * [Delete an image](#delete-an-image)
   * [Delete a container](#delete-a-container)
   * [Nuclear Clean :)](#nuclear-clean-)
      * [Clean all containers](#clean-all-containers)
      * [Clean all images](#clean-all-images)
* [Clean the volumes](#clean-the-volumes)
   * [List dangling volumes:](#list-dangling-volumes)
   * [List all volumes:](#list-all-volumes)
   * [Remove data within the volumes](#remove-data-within-the-volumes)
* [Docker Compose](#docker-compose)
   * [Useful commands for Docker Compose](#useful-commands-for-docker-compose)
* [Tag and Push your image to Dockerhub](#tag-and-push-your-image-to-dockerhub)


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
