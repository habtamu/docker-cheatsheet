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

** build command** used an docker file and generate out of it.
** . ** is a buld context (set of files and folders that belong to projects/encousulate in the container or used to build)
### 5.1 Tagging an image 
```console
docker build -t habka/redis:latest .
Note: your docker ID /. Repositor or project name : version
```
Note:
 - if the file name is Dockerfile.dev 
``` console
docker build -f Dockerfile.dev .
# To Run
docker run -it -p 3000:3000 d65829b5b674c98068d
```
## 6. Docker Volume

``` console
docker run -it -p 3000:3000 -v /app/node_modules -v $(pwd):/app  d65829b5b674c98068d

```
Note:
  - -v /app/node_module Put a bookmark on the node_module folder
  - -v $(pwd):/app Map the pwd into the '/app' folder
  
```
#Dockerfile.dev
FROM node:16-alpine

WORKDIR '/app'

COPY package.json .
RUN yarn install

COPY . .


CMD ["yarn", "run", "start"]

```
``` 
# docker-compose.yml
version: "3"
services:
  web:
    stdin_open: true
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - /app/node_modules
      - .:/app

```

## 7. Implementing multi step build
```
# docker file for production environment
# Build phase
#  - use node:alpine
#  - copy the package.json file
#  - install dependencies
#  - Run 'yarn run build'
# Run Phase
#  - use nginx
#  - copy over the result of 'yarn run build'
#  - start nginx 
# Finaly  
#  docker build .
#  docker run -p 8080:80 <image-id>

FROM node:16-alpine as builder
WORKDIR '/app'
COPY package.json .
RUN yarn install
COPY . .
RUN yarn run build

FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html
```
