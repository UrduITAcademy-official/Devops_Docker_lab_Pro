## Creating Nginx Image From Scratch Using a Dockerfile
Nginx is A powerful, advanced web server we can use to serve a variety of content, such as static web pages and APIs. In this section, we’ll begin by creating a new Dockerfile with the required instructions for building and running the Nginx image.

### 1.1. Initial Setup
First, let’s create a new directory and touch the Dockerfile:
````
mkdir server
````
````
touch Dockerfile
````
Now, we’re ready to populate our Dockerfile. Usually, the first command in the Dockerfile instructs Docker to create an image based on another image:
````
FROM ubuntu
````
Our Docker image will now be based on the minimal Ubuntu image, which is available on Dockerhub. The preceding commands in the Dockerfile will be layered on top of the base image.

### 1.2. Installing Nginx
In the next line, we’re going to tell Docker to install the nginx package from the official package repository using apt:
````
RUN apt-get -y update && apt-get -y install nginx
````
The command first tries to update the package repository. Once that’s successful, it installs the nginx package.

### 1.3. Configuration File
Nginx does come with a default configuration file. However, when we have our own Nginx configuration file, it’s much easier to maintain it outside of a Docker container. Therefore, we are going to create a default config file for Nginx in the same directory as the Dockerfile:
````
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    root /usr/share/nginx/html;
    index index.html index.htm;

    server_name _;
    location / {
        try_files $uri $uri/ =404;
    }
}
````
Let’s add an entry in the Dockerfile for copying our config file:
````
COPY default /etc/nginx/sites-available/default
````
Now, every time we build our Docker image, Docker copies the config file to the target directory.

### 1.4. Exposing Ports
Next, we’ll instruct the system to expose the port on which we’ll access our server. In our case, it’s going to be port 80 using TCP:
````
EXPOSE 80/tcp
````
### 1.5. Running Nginx
Now, we are going to run Nginx whenever our Docker image launches:
````
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
````
the -g option specifies a directive to nginx
the daemon off directive will disable the self-daemonizing behavior of nginx
We should note the ; at the end of the directive. Not specifying it might lead to unexpected problems.

With the final command, our whole Dockerfile now looks something like this:

````
# Pull the minimal Ubuntu image
FROM ubuntu

# Install Nginx
RUN apt-get -y update && apt-get -y install nginx

# Copy the Nginx config
COPY default /etc/nginx/sites-available/default

# Expose the port for access
EXPOSE 80/tcp

# Run the Nginx server
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
````

## 2. Running the Image
Once our image builds successfully, we are going to take it for a test run.

Let’s issue the following command to run the server:
````
docker run -d -p 80:80 haidar/server
````
Let’s break this down:

> the run sub-command specifies that we want to run the container
> the -d flag indicates that we want to run the image in detached mode
> the -p option signifies the port number in the format local-port:container-port. In this case, we are mapping port 80 in the container to port 80 on the server
> the final argument nginx specifies the tag of the image we want to run

Let’s fire up a browser and check our newly-built server:
![image](https://user-images.githubusercontent.com/71556060/201492262-4cb99a81-f81e-447e-aaeb-7429ddf44350.png)

