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
docker system prune
Note: to stop a container
```
