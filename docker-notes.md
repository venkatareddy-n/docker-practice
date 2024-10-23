Docker Notes
===============
Install Docker on RHEL Server

sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl start docker

sudo systemctl status docker

add ec2-user to the docker group

sudo usermod -aG docker ec2-user

exit and login again to get the effect

from one image we can create multiple images

docker images --> displays the images available in server

docker pull <image_name> --> pulls the image from the docker repository/dockerhub

docker pull nginx

docker pull nginx:alpine-slim  

docker create <image_name>:version

docker create nginx:alpine-slim --> create container from image 

docker create --name venkat d769f03e916d --> create a container from image with name

docker start container_ID --> start the container

docker stop container_ID --> stop the container

docker ps --> checks the running containers

docker ps -a --> checks the all containers

docker rm container_ID --> remove stopped container

docker rm -f container_ID --> remove running container

docker rmi image_ID --> remove image 

docker run <image_name>:version --> docker pull + docker create + docker start 

docker run nginx:alpine-slim --> it will run in background

-d --> for detach mode

docker run -d nginx:alpine-slim --> detaching to the screen, run in background

Port Forwarding
===============
-p for port forwarding
-p <host-port>:<container-port> --> port mapping

docker run -d -p 80:80 nginx:alpine-slim --> port 80 mapping --> ipaddress:80 to access nginx

docker run -d -p 81:80 nginx:alpine-slim --> port 81 mapping --> ipaddress:81 to access nginx

docker inspect <container-id>  --> to see container information

docker logs <container-id>  --> to see container logs

at the time of creating container you can give your own name to the container

docker run -d -p <host-port>:<container-port> --name <any_name> <docker_image>

docker run -d -p 81:80 --name venkat-test d769f03e916d

docker build --> for image creation
docker run --> for container creation

Dockerfile
==========
FROM 
-----

FROM almalinux:9

docker build -t <image_name>:<version> .  --> t for tag -- it will create docker image from Dockerfile
docker build -t from:v1 .

RUN
---

FROM almalinux:9
RUN dnf install nginx -y

docker build -t run:v1 .

docker images --> to see images

CMD
---
CMD instruction runs at the time of containse creation, its keeps containse running.

FROM almalinux:9
RUN dnf install nginx -y
CMD ["nginx", "-g", "daemon off;"]

docker build -t cmd:v1 .  --> it will create a image cmd:v1

docker run cmd:v1 --> creatgin a container from image cmd:v1 --> it will run in foreground

docker run -d cmd:v1 --> -d -> detached mode, runs in background

docker run -d --name nginx-server cmd:v1 --> --name -> name to a container 

docker run -d -p <host-port>:<container-port> --name <any_name> <docker_image>

docker run -d -p 80:80 --name nginx-2 cmd:v1

PUSH
=======
To push your docker image to your docker hub registry need to re-tag your images with your dockerid

docker tag cmd:v1 venkat2607/nginx:1.0

docker login -u docker_username
docker_password

docker push venkat2607/nginx:1.0 --> push your image to docker-hub

now we can use your docker image 

docker pull venkat2607/nginx:1.0  --> your docker image will download

RUN --> RUN will execute at the time of image creation

CMD --> CMD will execute at the time of container creation

LABEL
=====
label is used to find the proper image
FROM almalinux:9
LABEL author="venkat" \
      course="devops" \
      company="tcs"
LABEL duration="90 Days"

docker build -t label:v1 .

check the cocker images with tcs company

docker images -f 'label=company=tcs' --> it will display all tcs company images

docker inspect
--------------
docker inspect image_name   --> to see the image information

Execute - to enter inside the container
========
docker exec -it <container_id> bash or /bin/sh or sh

docker exec -it 09683c9dc855 /bin/sh
or
docker exec -it 09683c9dc855 bash
-----------------------------
EXPOSE
=======
The EXPOSE instruction doesn't actually publish the port.

FROM almalinux:9
RUN dnf install nginx -y
EXPOSE 80 
CMD ["nginx", "-g", "daemon off;"]

ENV
===
FROM almalinux:9
ENV course="devops" \
    trainse="siva"

ENV Sets the environment variables, these can be used inside the container
---------------

COPY
====
COPY is used to copy files from local to image

FROM almalinux:9
RUN dnf install nginx -y
RUN rm -rf /usr/share/nginx/html/index.html
COPY index.html /usr/share/nginx/html/index.html
CMD ["nginx", "-g", "daemon off;"]

docker build -t copy:v1 .  --> create image

docker run -d --name copy-server2 -p 80:80 copy:v1  --> create contaiser form image

docker exec -it 27d9d0b bash --> to enter inside the container

ADD
===
ADD also does the same as COPY, but it has 2 extra capabilities
1. It will download file form the internet
2. It can extract the files into the image

docker ps -a -q --> to list all containers

docker rm -f $(docker ps -a -q) --> remove all containers

ENTRYPOINT
==========
FROM almalinux:9
CMD ["ping", "google.com"]

docker build -t entry:v1 .

docker run -d entry:v1

docker logs -f <container_id> 

docker run entry:v1 ping yahoo.com --> CMD can replace at the time for container creation, CMD Can ne overridden, now ping yahoo.com will take
----------------------------
FROM almalinux:9
ENTRYPOINT ["ping", "google.com"]

docker build -t entry:v1 .

docker run -d entry:v1

docker logs -f <container_id> 

docker run -d entry:v1 ping yahoo.com

docker ps -a --no-trunc  --> to see the executed commands

ENTRYPOINT can't overriden like CMD, if you try to do it will go and append to the entrypoint command
command "ping google.com ping yahoo.com"
-------------------

FROM almalinux:9
CMD ["google.com"]
ENTRYPOINT ["ping"]

docker build -t entry:v1 .

docker run entry:v1 --> it will work

docker run entry:v1 yahoo.com --> it will work with yahoo

CMD will pass default arguments to ENTRYPOINT