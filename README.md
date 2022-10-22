### Devops_Docker_lab_Pro
# 🚀CONTAINERIZING AN APP FROM SCRATCH
The process of taking an application and configuring it to run as a container is called **“containerizing”**. Sometimes we call it **“Dockerizing”**.

![image](https://user-images.githubusercontent.com/71556060/196003045-e9c2872b-b5fb-4230-b19f-7e4ee8fe2c62.png)
The process of containerizing an app looks like this:
  - Start with your application code
  - Create a Dockerfile that describes your app, its dependencies, and how to run it
  - Feed this Dockerfile into the docker image build command to create an image
  - Create and run a container from that image
  
![image](https://user-images.githubusercontent.com/71556060/197335961-9d455359-054e-4236-bb01-4535cf7c045f.png)

## Dockerfile
Docker can build images automatically by reading the instructions from a Dockerfile.A Dockerfile is a simple text file with instructions on how to build your images
````

FROM nginx
COPY    .     /usr/share/nginx/html
````

Another Dockerfile example with more instruction for creating an image.This is a typical Dockerfile required for any **node.js** application.
````
FROM alpine
LABEL maintainer="aamirpinger@yahoo.com"
RUN apk add --update nodejs nodejs-npm
COPY      .     /src
WORKDIR /src
RUN npm install
ENV CREATEDBY="Aamir Pinger"
EXPOSE 8080
ENTRYPOINT ["node", "./app.js"]
````
## 🎯PUSHING IMAGES
One of the main advantage of image is portability, meaning you can use it from anywhere in the world. To achieve above you need to do only one thing, save your image to registry like **docker hub**.

This saving image is also referred as pushing image to the docker hub. You need to sign up for the account first at **https://hub.docker.com/**.

After creating an account you can simply push your image to docker hub repository. One thing important to know is for pushing an image to docker hub, we need our images to be built as **username/repository:tag**.
  - We have two options either we create a new image from same docker file with 
  ````
  docker build -t asadzoot/first-docker-app
  ````
  OR 
  - Second option is docker tag command which help make a new image from existing image with different name tags
  ````
  docker tag first-docker-app  asadzoot/first-docker-app
  ````
After proper tagging we need to do.
````
docker push asadzoot/first-docker-app
````
**Congratulation!** _You have uploaded your image to docker hub registry and your image is now portable_

## 📢INSPECTING IMAGE
To inspect layers of your image
````
docker history node-app-image
docker inspect node-app-image
````
## 🖇️BIND MOUNTS

By default all files created inside a container are stored on a writable container layer. This means that the data doesn’t persist when that container no longer exists, and it can be difficult to get the data out of the container if another process needs it.
![image](https://user-images.githubusercontent.com/71556060/196003682-0d7a42e7-1972-4c6c-b244-513634937045.png)

Docker has two options for containers to store files in the host machine, so that the files are persisted even after the container stops: 
  - Volumes,
  - Bind mounts

By using bind mount, a file or directory on the host machine is mounted into a container
````
docker run   -d  --name bindtest  -v ~/target-on-host:/app  nginx:latest
````
````
docker exec -it bindtest sh
# cd app
# echo “this is example file for docker bind” > test.txt
# ls
test.txt
# exit
````


Thank you and God bless you all!




