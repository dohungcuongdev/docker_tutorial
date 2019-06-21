**Important Docker Command**
docker pull [repository]        **pull repository**
docker run [image_name]         **run image**
docker images                   **list all images**
docker images -q                **list all id**
docker inspect [image_name]     **inspect image**
docker rmi [image_id]           **remove an image**
docker ps -a                    **list all containers**
docker ps                       **list all running containers**
docker history [image_id]       **see all the commands that were run with an image via a container.** 
docker history [image_name]     **see all the commands that were run with an image via a container.** 
docker top [container_id]       **show the top-level processes within a container**
docker stop [container_id]      **stop a running container**
docker rm [container_id]        **remove a container**
docker stats [container_id]     **provide the statistics of a running container**
docker attach [container_id]    **attach a running container**
docker pause [container_id]     **pause a running container**
docker unpause [container_id]   **unpause a running container**
docker kill [container_id]      **kill the processes in the container**
    **Note Begin**
    Initially, the Docker container will be in the created state.
    Then the Docker container goes into the running state when the Docker run command is used.
    The Docker kill command is used to kill an existing Docker container.
    The Docker pause command is used to pause an existing Docker container.
    The Docker stop command is used to pause an existing Docker container.
    The Docker run command is used to put a container back from a stopped state to a running state.
    **Note End**
service docker stop             **stop the Docker daemon process**
service docker start            **start the Docker daemon process**
docker network inspect bridge   **start the Docker daemon process**
docker build  -t [ImageName]:[TagName] [dir]
    **Note Begin**
    -t − is to mention a tag to the image
    [ImageName] − This is the name you want to give to your image.
    [TagName] − This is the tag you want to give to your image.
    [Dir] − The directory where the Docker File is present.
    **Note End**
docker rmi [image_id]  --force                  **sometime need to use force to delete**
docker rm $(docker ps -a -q -f status=exited)   **remove all containers where status=exited**
docker ps -a -q -f status=exited                **view all containers where status=exited**
docker ps -q |xargs docker rm
docker images -q |xargs docker rmi
**Note Begin**
My favorite way of removing all stopped docker containers is:
docker ps -q |xargs docker rm
it will list all images (docker ps), but only show the id’s. And then run a docker rm command for each one of them.
Similarly for removing all unused docker images
docker images -q |xargs docker rmi
Will list all docker images and then docker rmi them.
**Note End**

#Cannot delete image try this
docker rmi [image_id] --force

#Can’t delete docker image with dependent child images, try this
docker rm $(docker ps -aq)

docker run [OPTIONS] [IMAGE] [COMMAND] [ARG...]
**Example docker run command**
docker run -p 80:8080 --name kitura -d ibmcom/kitura-ubuntu
docker run -p 90:8080 --rm --name kitura2 ibmcom/kitura-ubuntu
docker run -p 8083:8083 -t springio/spring-boot-docker

# Public Repositories
docker build -t [Repositoryname]:[tag] .
docker push [Repositoryname]:[tag]
docker pull [Repositoryname]:[tag]
docker tag [imageID] [Repositoryname]
docker push [Repositoryname]:latest
# Go to https://hub.docker.com/ , create repository: myfirstimage under username: dohungcuongdev
# Make a Dockerfile on local and build image with it
docker build -t dohungcuongdev/myfirstimage:0.1 .
docker push dohungcuongdev/myfirstimage:0.1
docker pull dohungcuongdev/myfirstimage:0.1
docker tag 8671e8b30bf7 dohungcuongdev/myfirstimage
docker push dohungcuongdev/myfirstimage:latest
**8671e8b30bf7 should be the id of an image and could be different from image [dohungcuongdev/myfirstimage:0.1]**

**Important Instruction Command**
FROM        **The base image for building a new image. This command must be on top of the dockerfile.**
MAINTAINER  **Optional, it contains the name of the maintainer of the image.**
RUN         **Used to execute a command during the build process of the docker image.**
ADD         **Copy a file from the host machine to the new docker image. There is an option to use an                 URL for the file, docker will then download that file to the destination directory.**
ENV         **Define an environment variable.**
CMD         **Used for executing commands when we build a new container from the docker image.**
ENTRYPOINT  **Define the default command that will be executed when the container is running.**
WORKDIR     **This is directive for CMD command to be executed.**
USER        **Set the user or UID for the container created with the image.**
VOLUME      **Enable access/linked directory between the container and the host machine.**


**Some Concept**

1. Docker Architecture:
The following image shows the new generation of virtualization that is enabled via Dockers. Let’s have a look at the various layers.

APP      APP      App
    Docker Engine
       Host OS
       Server

The server is the physical server that is used to host multiple virtual machines. So this layer remains the same.
The Host OS is the base machine such as Linux or Windows. So this layer remains the same.
Now comes the new generation which is the Docker engine. This is used to run the operating system which earlier used to be virtual machines as Docker containers.
All of the Apps now run as Docker containers.
The clear advantage in this architecture is that you don’t need to have extra hardware for Guest OS. Everything works as Docker containers.

2. Docker image and container:
A Docker image is a file, comprised of multiple layers, used to execute code in a Docker container. An image is essentially built from the instructions for a complete and executable version of an application, which relies on the host OS kernel. When the Docker user runs an image, it becomes one or multiple instances of that container.
A Docker image is made up of multiple layers. A user composes each Docker image to include system libraries, tools, and other files and dependencies for the executable code. Image developers can reuse static image layers for different projects. Reuse saves time, because a user does not have to create everything in an image.
An instance of an image is called a container. You have an image, which is a set of layers as you describe. If you start this image, you have a running container of this image. You can have many running containers of the same image.
You can see all your images with docker images whereas you can see your running containers with docker ps (and you can see all containers with docker ps -a).
So a running instance of an image is a container.

3. The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile.
You dont have to
RUN mkdir -p /usr/src/app
This will be created automatically when you specifiy your WORKDIR
WORKDIR /usr/src/app

4. Sometimes you see COPY or ADD being used in a Dockerfile, but 99% of the time you should be using COPY, here's why.
COPY and ADD are both Dockerfile instructions that serve similar purposes. They let you copy files from a specific location into a Docker image.
COPY takes in a src and destination. It only lets you copy in a local file or directory from your host (the machine building the Docker image) into the Docker image itself.
ADD lets you do that too, but it also supports 2 other sources. First, you can use a URL instead of a local file / directory. Secondly, you can extract a tar file from the source directly into the destination.
In most cases if you’re using a URL, you’re downloading a zip file and are then using the RUN command to extract it. However, you might as well just use RUN with curl instead of ADD here so you chain everything into 1 RUN command to make a smaller Docker image.
A valid use case for ADD is when you want to extract a local tar file into a specific directory in your Docker image. This is exactly what the Alpine image does with ADD rootfs.tar.gz /.
If you’re copying in local files to your Docker image, always use COPY because it’s more explicit.


**References**
https://www.tutorialspoint.com/docker/
https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index
https://www.raywenderlich.com/9159-docker-on-macos-getting-started
https://www.howtoforge.com/tutorial/how-to-create-docker-images-with-dockerfile/
