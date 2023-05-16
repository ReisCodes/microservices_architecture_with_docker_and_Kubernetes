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

