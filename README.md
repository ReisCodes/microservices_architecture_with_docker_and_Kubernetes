# Microservice Architecture with Docker and K8's

#### What is a Microservice?

Microservices are an architectural and organizational approach to software development where software is composed of small independent services that communicate over well-defined APIs and described as "loosely coupled". These services are owned by small, self-contained teams.

Microservices architectures make applications easier to scale and faster to develop, enabling innovation and accelerating time-to-market for new features.

#### Difference between N-Tier Architecture and Microservices?

The N-tier architecture is simple to deploy but rigid in its design to support continuous delivery of new capabilities. Microservices architecture increases operational complexity but is flexible in its design to enable continuous integration and delivery of new capabilities.

#### What is Docker? and Containerisation

Docker is a Software used to build test and deploy applications quickly. It does this by packaging everything the software needs to run (In Containers) so it can be easily shared and replicated on different OS as long as they have docker installed.

#### Virtaulisation vs Containerisation

They essentially both want to achieve the same goal create environmets to build, test and deploy applications but containerisation tries to eliminate the problem of works on my laptop by eliminating the OS needs. virtualisation is optimal in certain situations as they can be quickly deployed, as the underlying OS and dependencies are already loaded on the hypervisor.

#### What is Kuberenetes (K8's)?

Kubernetes is an open-source container orchestration system for automating software deployment, scaling, and management.

![](MS_arch.png)

![](docker.png)


### Running docker images from terminal 

1. To get nginx image from docker hub to run on port 80 from 80 we use the command: 

```
docker run -d -p 80:80 nginx
```

2. We can see on our terminal the images, and those being run:

```
docker images

docker ps
```

3. To stop a container we use 

```
docker stop <container_id>
```

4. To start a container back up again we use

```
docker start <container_id>
```

5. To remove a container image 

```
docker rm <container_id> -f
```

### Connecting to your container "instance"

1. To connect into the container to apply changes/updates etc., we also have to do some seperate updates as its a new instance for sudo commands

```
docker exec -it a11073e1fc79 /bin/bash
```

2. Once connected we can navigate to the html directory 

```
cd /usr

cd share

cd nginx 

cd html
```

3. We can confirm the relevant files are present with:

```
ls
```

4. we can the perform these updates and downloads to ammend the HTML file.

```
apt update -y 

apt install sudo 

sudo apt install nano 
```

5. We can then edit the HTML file

```
sudo nano index.html
```

Edit this file, save it.
Refresh the browser and see updated HTML page 

![](nginx.png)

### Creating our own docker image 

1. firstly need to create a file locally named `index.html`, within this add the context youd like to be displayed 

```
<html>
  <head>
    <title>Dockerfile</title>
  </head>
  <body>
    <div class="container">
      <h1>Reis Pinnock</h1>
      <h2>This is my SECOND time creating a docker image</h2>
      <p>Hello everyone, This is running via Docker container</p>
    </div>
  </body>
</html>
```

2. From the same directory we need to docker copy this file into our nginx image

```
docker cp index.html <docker_image_id>:usr/share/nginx/html
```
3. we commit these changes to the image with 

```
docker commit <image_ID> <image_name>
```

4. we then need to tag this image before we can push this with:

```
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

5. we then need to create a repo on docker hub 

<br>

6. now we can push to the repo with

```
docker push <repo_name>/<image_name>:tag
```

#### Automating the creation of image

1. change directory location to where the `index.html` 

2. Create a new `Dockerfile`

```
sudo nano Dockerfile
```

3. Within this we need to add the relevant info

```
# base image Nginx
FROM nginx:latest

# Who is the owner/creater
LABEL MAINTAINER-Reis-Pinnock

# copy index.html to /usr/share/nginx/html
COPY index.html to /usr/share/nginx/html/

# use port 80
EXPOSE 80

# execute command run
CMD ["nginx", "-g", "daemon off;"]
```

4. Create the image 

```
sudo docker build -t <repo_name>/<image_name>:tag .
```

5. We can verify on localhost with 

```
docker run -d -p 90:80  <repo_name>/<image_name>:tag
```

Navigate to the localhost and should see ammended web page.

6. We can push this image to `Docker hub` 

```
docker push  <repo_name>/<image_name>:tag
```

#### Automating building image for node app

1. navigate to the directory where our app folder is and create a `Dockerfile`

```
sudo nano Dockerfile
```

2. Within this add the following script

```
# base image Node
FROM node:16

# Create app directory
WORKDIR /app

# Who is the owner
LABEL MAINTAINER=Reis-Pinnock

# Copy app to work dir
COPY app/ .

# use port 3000
EXPOSE 3000

# install npm
RUN npm install

# excecute commands
CMD ["npm", "start", "daemon off;"]
```

3. we can now build the image:

```
docker build -t reiscodes/sparta_app:v2 .
```

4. we can verify this locally with 

```
docker run -d -p 3000:3000 reiscodes/sparta_app:v2
```

5. Once we can see it running locally we can push this image to docker hub with:

```
docker push reiscodes/sparta_app:v2
```

