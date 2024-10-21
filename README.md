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

docker ps --> checks the running containers

docker ps -a --> checks the all containers

docker pull <image_name> --> pulls the image from the docker repository/dockerhub

docker pull nginx

docker pull nginx:alpine-slim  

docker create <image_name>:version

docker create nginx:alpine-slim --> create container from image 

docker start container_ID --> start the container

docker stop container_ID --> stop the container

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

Execute 
========
docker exec -it <container_id> bash or /bin/sh or sh

docker exec -it 09683c9dc855 /bin/sh

docker exec -it 09683c9dc855 bash

docker inspect <container-id>  --> to see container information

docker logs <container-id>  --> to see container logs

at the time of creating container you can give your own name to the container

docker run -d -p <host-port>:<container-port> --name <any_name> <docker_image>

docker run -d -p 81:80 --name venkat-test d769f03e916d

Dockerfile
==========

docker build -t <image_name>:<version> .  --> t for tag -- it will create docker image from Dockerfile

docker run -d -p <host-port>:<container-port> --name <your_cont_name> <image_name>:<version> -- it will create container from your docker image

