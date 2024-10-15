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



