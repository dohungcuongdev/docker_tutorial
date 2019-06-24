#How can I inspect the file system of a failed `docker build`?
Everytime docker successfully executes a RUN command from a Dockerfile, a new layer in the image filesystem is committed. Conveniently you can use those layers ids as images to start a new container.

Take the following Dockerfile:

FROM busybox
RUN echo 'foo' > /tmp/foo.txt
RUN echo 'bar' >> /tmp/foo.txt
and build it:

$ docker build -t so-2622957 .
Sending build context to Docker daemon 47.62 kB
Step 1/3 : FROM busybox
 ---> 00f017a8c2a6
Step 2/3 : RUN echo 'foo' > /tmp/foo.txt
 ---> Running in 4dbd01ebf27f
 ---> 044e1532c690
Removing intermediate container 4dbd01ebf27f
Step 3/3 : RUN echo 'bar' >> /tmp/foo.txt
 ---> Running in 74d81cb9d2b1
 ---> 5bd8172529c1
Removing intermediate container 74d81cb9d2b1
Successfully built 5bd8172529c1
You can now start a new container from 00f017a8c2a6, 044e1532c690 and 5bd8172529c1:

$ docker run --rm 00f017a8c2a6 cat /tmp/foo.txt
cat: /tmp/foo.txt: No such file or directory

$ docker run --rm 044e1532c690 cat /tmp/foo.txt
foo

$ docker run --rm 5bd8172529c1 cat /tmp/foo.txt
foo
bar
of course you might want to start a shell to explore the filesystem and try out commands:

$ docker run --rm -it 044e1532c690 sh      
/ # ls -l /tmp
total 4
-rw-r--r--    1 root     root             4 Mar  9 19:09 foo.txt
/ # cat /tmp/foo.txt 
foo
When one of the Dockerfile command fails, what you need to do is to look for the id of the preceding layer and run a shell in a container created from that id:

docker run --rm -it <id_last_working_layer> bash -il
Once in the container:

try the command that failed, and reproduce the issue
then fix the command and test it
finally update your Dockerfile with the fixed command
If you really need to experiment in the actual layer that failed instead of working from the last working layer, see Drew's answer.


#Check file when build image failed:
docker exec -it <container-name> /bin/bash
docker exec -it <container-name> sh

docker exec -it <container-name> bash
ls -a


#References:
1. https://stackoverflow.com/questions/26220957/how-can-i-inspect-the-file-system-of-a-failed-docker-build

2. https://stackoverflow.com/questions/20813486/exploring-docker-containers-file-system