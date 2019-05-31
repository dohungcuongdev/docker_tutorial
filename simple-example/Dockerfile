#This is a sample Image 
FROM ubuntu
MAINTAINER dohungcuongdev1295@gmail.com
RUN apt-get update
#the following line doesnot work on MAC OS:
#RUN apt-get install –y nginx 
CMD ["echo","Image created"]

#The first line "#This is a sample Image" is a comment. You can add comments to the Docker File with the help of the # command
#The next line has to start with the FROM keyword. It tells docker, from which base image you want to base your image from. In our example, we are creating an image from the ubuntu image.
#The next command is the person who is going to maintain this image. Here you specify the MAINTAINER keyword and just mention the email ID.
#The RUN command is used to run instructions against the image. In our case, we first update our Ubuntu system and then install the nginx server on our ubuntu image.
#The last command is used to display a message to the user

#To build the image: 
# sudo docker build -t myimage:0.1 .
#Here, myimage is the name we are giving to the Image and 0.1 is the tag number we are giving to our image.
#Since the Docker File is in the present working directory, we used "." at the end of the command to signify the present working directory.

#Public Repositories
# Go to https://hub.docker.com/ , create repository: myfirstimage under username: dohungcuongdev
# docker build -t dohungcuongdev/myfirstimage:0.1 .
# docker push dohungcuongdev/myfirstimage:0.1
# docker pull dohungcuongdev/myfirstimage:0.1