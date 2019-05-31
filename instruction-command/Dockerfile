FROM ubuntu 
MAINTAINER dohungcuongdev1295@gmail.com
#CMD ["echo" , "hello world"]
#ENTRYPOINT ["echo"]
#ENV var1=Tutorial var2=point
WORKDIR /newtemp
CMD pwd


#Build container - test CMD - just keep line 1,2,3
#   docker build -t ici .
#   docker run ici
# result: pirnt out hello world

#Build  container - test ENTRYPOINT - just keep line 1,2,4
#   docker build -t ici .
#   docker run ici hello
# result: pirnt out hello

#Build  container - test ENV - just keep line 1,2,5 - after enter command, screen will display root@a7ef26e161b4:/#, then enter env to check the ENV value
#   docker build -t ici .
#   docker run -it ici /bin/bash
# result: root@a7ef26e161b4:/# -> env -> var1 and var2 will be displayed in the list

#Build container - test WORKDIR - just keep line 1,2,6,7 - set the working directory of the container, If the directory does not exist, it will be added.
#   docker build -t ici .
#   docker run ici
# result: pirnt out /newtemp



