# Useful notes about Docker






## Clean images adn containers

### Nuclear Clean :)

> If you get an error like **"no space left on device"** and if you don't care about doing a *nuclear clean*, you can:

#### Clean all containers

```shell
docker ps -a | sed '1 d' | awk '{print $1}' | xargs -L1 docker rm
```

#### Clean all images 

> Again, careful when using such commands, do not use it in production. The great thing is that if you have your Dockerfile(s), it's easy to rebuild and or docker pull.

```shell
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
