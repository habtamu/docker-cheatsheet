# docker-cheatsheet
## 1. Creating and Running a container from an image
```console
# Run command (docker run = docker create + docker start)
docker run <image name>
Example: docker run hello-world
```
### 1.1 Creating a docker
```console
docker create <image name>
Example: docker create hello-world
```
### 1.2 Starting a docker
```console
docker start -a <container-id>
Example: docker start -a 08d23fa02c1a
-a means watch the output and print on the terminal
```
### 1.1 override a docker run command
```console
docker run <image name> command!
Example: docker run busybox ls
```
## 2. List all running containers
```console
docker ps
docker ps --all
Example
docker run busybox ping google.com
CONTAINER ID   IMAGE     COMMAND             CREATED         STATUS         PORTS     NAMES
08d23fa02c1a   busybox   "ping google.com"   6 seconds ago   Up 5 seconds             keen_brahmagupta
```
### 2.1 Get logs from a container
```console
docker logs <container id>
Example: docker start 08d23fa02c1a
         docker logs 08d23fa02c1a
Note: Logs didnt rerun the docker except logs emitted from that container
```
## 3. stop and distroy containers
```console
docker system prune
Note: to distory all containers
```
```console
docker stop <container id>
Note: stop command send terminate signal to the process and the process stops on its time
docker kill <container id>
Note: send kill signal and shutdown immediately no additional work.
```
## 4. Container
### 4.1 Execute an additional command in a container
```console
docker exec -it <container id> <command>
Example: docker exec -it <container id> redis-cli
Note: create a redis server container and run the above command
exec (run another command) -it (input)
```
### 4.2 start shell script
```console
exec -it <container id> <command>
Example: docker exec -it <container id> sh
Example: docker run -it busybox sh
Note: Comman processors: sh, bash, powershell,zsh
```
## 5. Buiding a custom images
### 5.1 Create an image that run redis-server
```console
- create DockerFile
# Use an existing docker image as a base
FROM alpine
# Download and install a dependency
RUN apk add  --update redis
# Tell the image what to do when it starts as a container
CMD ["redis-server"]
# go to shell> docker build .
```


