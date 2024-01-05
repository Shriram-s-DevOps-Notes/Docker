# *Docker file*

- Docker can build images automatically by reading the instructions from a Dockerfile. 
- A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. 
- The format of the Docker file is similar to the below syntax:

INSTRUCTION arguments

- A Docker file must start with a **FROM** instruction.

- The FROM instruction specifies the Base Image from which you are building.

Multiple INSTRUCTIONS are available in the Docker file, some of these include:
                              
| Instruction   | Description                                                          |
|---------------|----------------------------------------------------------------------|
| ADD           | Add local or remote files and directories.                          |
| ARG           | Use build-time variables.                                            |
| CMD           | Specify default commands.                                            |
| COPY          | Copy files and directories.                                          |
| ENTRYPOINT    | Specify default executable.                                          |
| ENV           | Set environment variables.                                           |
| EXPOSE        | Describe which ports your application is listening on.              |
| FROM          | Create a new build stage from a base image.                          |
| HEALTHCHECK   | Check a container's health on startup.                               |
| LABEL         | Add metadata to an image.                                            |
| MAINTAINER    | Specify the author of an image.                                      |
| ONBUILD       | Specify instructions for when the image is used in a build.         |
| RUN           | Execute build commands.                                              |
| SHELL         | Set the default shell of an image.                                   |
| STOPSIGNAL    | Specify the system call signal for exiting a container.              |
| USER          | Set user and group ID.                                               |
| VOLUME        | Create volume mounts.                                                |
| WORKDIR       | Change the working directory.  

- To know more about the docker file parameters visit **https://docs.docker.com/engine/reference/builder/**

- An example of a docker image is shown below
1. Create a directory
2. cd to that directory
3. Create an file called ```dockerfile``` add below contents
```
ARG version=latest
FROM ubuntu:${version}
RUN apt-get update -y && apt-get install -y nginx
COPY index.html /usr/share/nginx/html
EXPOSE 80 8000
CMD ["nginx", "-g", "daemon off;"]
```
4. Create index.html file in the same directory
5. For the final step run
```docker image build -t <image-name>:<image-tag> .```

- Example: ```docker image build -t custom_nginx:v1 .```

6. Verify whether the image is created or not by running ``` docker images```

```
root@ip-172-31-16-115:~/docker-image-1# docker images                                                                                           
REPOSITORY     TAG       IMAGE ID       CREATED          SIZE
custom_nginx   v1        6921e4951930   11 seconds ago   180MB
mysql          latest    73246731c4b0   2 weeks ago      619MB
mariadb        latest    c74611c2858a   2 weeks ago      404MB
alpine         latest    f8c20f8bbcb6   3 weeks ago      7.38MB
nginx          latest    d453dd892d93   2 months ago     187MB
httpd          latest    6fd77d7e5eb7   2 months ago     167MB
centos         7         eeb6ee3f44bd   2 years ago      204MB
ubuntu         14.04     13b66b487594   2 years ago      197MB
```
- Now you can create a container using this image.
### *Difference between copy and add:*
---
- COPY takes in a source and destination. It only lets you copy in a local file or directory from your host

ADD lets you do that too, but it also supports 2 other sources.

1. First, you can use a URL instead of a local file/directory.
2. Secondly, you can extract a tar file from the source directly into the destination

- Basically, the ADD command will unzip and add it to the destination path.

- Even though the add command will decompress the file it is recommended to use the curl command.

- Using ADD to fetch packages from remote URLs is strongly discouraged; you should use curl or wget instead.

Below, we will get 3 layers of work that will increase the container size.
```
ADD http://example.com/big.tar.xz /usr/src/things/
RUN tar -xJf /usr/src/things/big.tar.xz -C /usr/src/things
RUN make -C /usr/src/things all
```
