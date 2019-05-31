FROM ubuntu 
RUN apt-get update 
#the following line doesnot work on MAC OS:
#RUN apt-get install –y apache2 
#RUN apt-get install –y apache2-utils 
RUN apt-get clean 
EXPOSE 80 
CMD [“apache2ctl”, “-D”, “FOREGROUND”]


# docker build -t="mywebserver" .
# docker run –d –p 80:80 mywebserver