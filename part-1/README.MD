## PART 1

### 1.1: Getting started

> Start 3 containers from image that does not automatically exit, such as nginx, detached. Stop 2 of the containers leaving 1 up. Submit the output for docker ps -a which shows 2 stopped containers and one running.

```
docker container run -d nginx 
docker container run -d nginx
docker container run -d nginx

docker container stop 9ac e3f

docker ps -a
```
![1.1: Getting started](../assets/exercise-1-1.png)

### 1.2: Cleanup

> Clean the docker daemon from all images and containers.

```
docker container stop ef6
docker container rm 9ac e3f ef6

docker ps -a
docker images
```
![1.2: Cleanup](../assets/exercise-1-2.png)

### 1.3: Secret message

> Image devopsdockeruh/simple-web-service:ubuntu will start a container that outputs logs into a file. Go inside the container and use tail -f ./text.log to follow the logs. Every 10 seconds the clock will send you a “secret message”.

```
docker run -d -it --name get-secret devopsdockeruh/simple-web-service:ubuntu
docker exec -it get-secret bash 
```
inside the container run
```
tail -f ./text.log
```
Secret message is: 'You can find the source code here: https://github.com/docker-hy'

![1.3: Secret message](../assets/exercise-1-3.png)

### 1.4: Missing dependencies

>Start a ubuntu image with the process sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;' You will notice that a few things required for proper execution are missing. Be sure to remind yourself which flags to use so that the read actually waits for input.

```
docker run -d -it --name website-helsinki ubuntu sh -c 'echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'

docker exec -it website-helsinki bash
```
inside the container run
```
apt-get update
apt-get install curl
exit
```

```
docker start -i -a website-helsinki
Input website: helsinki.fi
```

![1.4: Missing dependencies](../assets/exercise-1-4.png)

### 1.5: Sizes of images

>In a previous exercise we used devopsdockeruh/simple-web-service:ubuntu. Here is the same application but instead of ubuntu is using alpine: devopsdockeruh/simple-web-service:alpine. Pull both images and compare the image sizes. Go inside the alpine container and make sure the secret message functionality is the same. Alpine version doesn’t have bash but it has sh.

```
docker pull devopsdockeruh/simple-web-service:alpine
docker images 
```
![1.5: Sizes of images](../assets/exercise-1-5.png)

```
docker run -d -it --name get-secret-alpine devopsdockeruh/simple-web-service:alpine
docker exec -it get-secret-alpine sh 
```
inside the container run
```
tail -f ./text.log
```
Secret message is: 'You can find the source code here: https://github.com/docker-hy'

![1.5: Secret](../assets/exercise-1-5-2.png)

### 1.6: Hello Docker Hub

>Run docker run -it devopsdockeruh/pull_exercise. It will wait for your input. Navigate through docker hub to find the docs and Dockerfile that was used to create the image. Read the Dockerfile and/or docs to learn what input will get the application to answer a “secret message”.

```
docker run -it devopsdockeruh/pull_exercise
```
Secret message is: "This is the secret message"
![1.6: Hello Docker Hub](../assets/exercise-1-6.png)

### 1.7: Two line Dockerfile
> Create a Dockerfile and use FROM and CMD to create a brand new image that automatically runs the server. Tag the new image as “web-server”

[Dockerfile](./exercise-1-7/Dockerfile)

```
docker build . -t web-server
docker run web-server
```
### 1.8: Image for script
> Create a Dockerfile for a new image that starts from ubuntu:18.04. Make a script file on you local machine with such content as echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;. Transfer this file to an image and run it inside the container using CMD. Build the image with tag “curler”. Run the new curler image with the correct flags and input helsinki.fi into it. Output should match the 1.4 one.

[Dockerfile](./exercise-1-8/Dockerfile)

```
docker build . -t curler
docker run -it curler   
```

### 1.9: Volumes
> Image devopsdockeruh/simple-web-service creates a timestamp every two seconds to /usr/src/app/text.log when it’s not given a command. Start the container with bind mount so that the logs are created into your filesystem.

```
mkdir mydir
touch mydir/text.log
docker run -v $(pwd)/mydir/text.log:/usr/src/app/text.log devopsdockeruh/simple-web-service:ubuntu
```
[Output text](./mydir/text.log)


### 1.10: Ports open
> Image devopsdockeruh/simple-web-service will start a web service in port 8080 when given the command “server”. From 1.7 you should have an image ready for this. Use -p flag to access the contents with your browser. The output to your browser should be something like: { message: "You connected to the following path: ...

```
docker run -p 8080:8080  web-server
```
output of browser
```json
{
"message": "You connected to the following path: /",
"path": "/"
}
```

### 1.11: Spring
> Lets create a Dockerfile for a Java Spring project: github page. The setup should be straightforward with the README instructions. Tips to get you started: Use openjdk image FROM openjdk:_tag_ to get java instead of installing it manually. Pick the tag by using the README and dockerhub page.

```
docker build . -t spring-project
docker run -p 8080:8080 spring-project
```
[Dockerfile](./exercise-1-11/Dockerfile)

### 1.12: Hello, frontend!
> Clone, fork or download the project from https://github.com/docker-hy/material-applications/tree/main/example-frontend. Create a Dockerfile for the project (example-frontend) and give a command so that the project runs in a docker container with port 5000 exposed and published so when you start the container and navigate to http://localhost:5000 you will see message if you’re successful.

```
docker build . -t example-frontend
docker run -p 5000:5000  example-frontend
```
[Dockerfile](./exercise-1-12/Dockerfile)

message of browser:
Exercise 1.12: Congratulations! You configured your ports correctly!

### 1.13: Hello, backend!
> Clone, fork or download a project from https://github.com/docker-hy/material-applications/tree/main/example-backend. Create a Dockerfile for the project (example-backend) and give a command so that the project runs in a docker container with port 8080 published. When you start the container and navigate to http://localhost:8080/ping you should get a “pong” as response.

```
docker build . -t example-backend
docker run -p 8080:8080  example-backend
```
[Dockerfile](./exercise-1-13/Dockerfile)

message of http://localhost:8080/ping: pong
