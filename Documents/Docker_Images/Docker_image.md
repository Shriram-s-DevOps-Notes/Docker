# Docker Images
----
- **An image is a read-only template with instructions for creating a Docker container.** Often, an image is based on another image, with some additional customization. For example, you may build an image that is based on the Ubuntu image but installs the Apache web server and your application, as well as the configuration details needed to make your application run.
### What is a docker image?
- Official definition: **"An Image is an ordered collection of root
filesystem changes and the corresponding execution
parameters for use within a container runtime."**

- Docker image consists of the following things
1. App binaries and dependencies
2. Metadata about the image data and how to run the image
3. Small as one file (your app binary) like a golang static binary
4. Big as a Ubuntu distro with apt, Apache, PHP, and more
installed

- Not a complete OS. No kernel, kernel modules (e.g. drivers)

### Difference Between Docker Containers and Docker Images:
---
- The major difference between a container and an image is the top writable layer.

- All writes to the container that add new or modify existing data are stored in this writable layer.

![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/b5df24c0-9135-4292-a905-8d79e109d8cc)



### Docker hub
---
Docker Hub is a cloud-based repository service where users can create, test, store, and distribute container images. It's a part of Docker's suite of cloud services for building and sharing containerized applications and microservices. Docker Hub allows developers to:

- Store and Share Images
- Automated Builds
- Public and Private Repositories
- Integration with Docker Desktop and CLI
- Community and Official Images
- Version Control and Tagging
- Teams and Collaboration

**Types of docker hub repository**
1. Public Repositories:
2. Private Repositories: 
* Docker Hub offers both public and private storage for images. Public repositories are accessible to everyone, while private repositories offer more control and privacy for proprietary applications.

- To create the docker hub repo click on the link **https://hub.docker.com/**

- Once the account is created login to docker hub.

- You can log in to the docker hub through your machine terminal. To log in run the command
```
docker login
```
- Add username and password.
- Suppose if I want to download the older version of Python all I need open the docker hub & search for python
Right side in recent tags copy the version tag code and paste it in the docker cli as mentioned below

![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/efdffec9-2292-4a75-a4bc-b85ca55741d3)

![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/63b9ae96-9665-4601-9a9d-34263f621b9e)

- Run
```
Docker pull python:3.12.0b1
```
### Docker hub in CLI
---
Using Docker CLI (Command Line Interface) to interact with Docker Hub allows you to perform various operations like searching, pulling, and pushing images. However, when it comes to filtering results directly in the CLI, the functionality is somewhat limited compared to the web interface of Docker Hub.

Here are some common ways you can use Docker CLI with some level of filtering:

1. **Searching Images**: You can search for images on Docker Hub using the `docker search` command. This command allows basic filtering based on the search string.

   ```bash
   docker search [OPTIONS] TERM
   ```

   For example, `docker search ubuntu` will list Docker images related to Ubuntu. However, this search is limited to the image names and descriptions.

2. **Filtering by Stars**: You can use the `--filter` option with `docker search` to filter the search results based on the number of stars. For example, to find Ubuntu images with at least 50 stars:

   ```bash
   docker search --filter stars=50 ubuntu
   ```

3. **Automated Builds**: You can filter the search to show only images with automated builds:

   ```bash
   docker search --filter=is-automated=1 ubuntu
   ```

4. **Official Images**: To filter your search to only show official images:

   ```bash
   docker search --filter=is-official=1 ubuntu
   ```

5. **Limiting Results**: You can limit the number of displayed results using the `--limit` option:

   ```bash
   docker search --limit 10 ubuntu
   ```

6. **Using Format for Custom Output**: The `--format` option allows you to define a custom output format, which can be useful for parsing the results:

   ```bash
   docker search --format "{{.Name}}: {{.Description}}" ubuntu
```

### *Docker Image layers*
---
- Every Docker image consists of read-only layers. These layers represent the Docker image file system and are stacked on top of each other. Each layer is a set of differences from the layer before it.

- When a container is created from an image, Docker adds a new writable layer on top of the read-only layers. This writable layer is often referred to as the "container layer." All changes made to the running container - such as writing new files, modifying existing files, and deleting files - are written to this writable layer.

- The read-only layers of an image remain unchanged throughout the lifecycle of the image. When multiple containers are spawned from the same image, they all share the image's read-only layers but have their own individual writable layers.

- This architecture helps to efficiently manage storage space and enables quick start-up times for containers, as only the new writable layer needs to be created for each new container.

- To check the image layers run command
```
docker history <image-name>:<image-tag>
```
Example: Below is an example for Image layers. Official HTTPD package has 15 Layers.
```
root@ip-172-31-16-115:~# docker history httpd:latest
IMAGE          CREATED        CREATED BY                                      SIZE      COMMENT
6fd77d7e5eb7   2 months ago   CMD ["httpd-foreground"]                        0B        buildkit.dockerfile.v0
<missing>      2 months ago   EXPOSE map[80/tcp:{}]                           0B        buildkit.dockerfile.v0
<missing>      2 months ago   COPY httpd-foreground /usr/local/bin/ # buil…   138B      buildkit.dockerfile.v0
<missing>      2 months ago   STOPSIGNAL SIGWINCH                             0B        buildkit.dockerfile.v0
<missing>      2 months ago   RUN /bin/sh -c set -eux;   savedAptMark="$(a…   81.7MB    buildkit.dockerfile.v0
<missing>      2 months ago   ENV HTTPD_PATCHES=                              0B        buildkit.dockerfile.v0
<missing>      2 months ago   ENV HTTPD_SHA256=fa16d72a078210a54c47dd5bef2…   0B        buildkit.dockerfile.v0
<missing>      2 months ago   ENV HTTPD_VERSION=2.4.58                        0B        buildkit.dockerfile.v0
<missing>      2 months ago   RUN /bin/sh -c set -eux;  apt-get update;  a…   11MB      buildkit.dockerfile.v0
<missing>      2 months ago   WORKDIR /usr/local/apache2                      0B        buildkit.dockerfile.v0
<missing>      2 months ago   RUN /bin/sh -c mkdir -p "$HTTPD_PREFIX"  && …   0B        buildkit.dockerfile.v0
<missing>      2 months ago   ENV PATH=/usr/local/apache2/bin:/usr/local/s…   0B        buildkit.dockerfile.v0
<missing>      2 months ago   ENV HTTPD_PREFIX=/usr/local/apache2             0B        buildkit.dockerfile.v0
<missing>      2 months ago   /bin/sh -c #(nop)  CMD ["bash"]                 0B        
<missing>      2 months ago   /bin/sh -c #(nop) ADD file:ac3cd70031d35e46d…   74.8MB    
root@ip-172-31-16-115:~# 
```
- To know the metadata of the one image run the command
```
docker image inspect <image-name>
```
Example:
```
root@ip-172-31-16-115:~# docker image inspect httpd
[
    {
        "Id": "sha256:6fd77d7e5eb732dacab601d4556c04a6c312928fb8989fe3b0a47d82db772441",
        "RepoTags": [
            "httpd:latest"
        ],
        "RepoDigests": [
            "httpd@sha256:f0a93744d8006e6f7ee5086c9ddccdcfa33d1091f15269a00547b4c382459c1f"
        ],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2023-10-19T11:31:14Z",
        "Container": "",
        "ContainerConfig": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": null,
            "Cmd": null,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "DockerVersion": "",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/apache2/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "HTTPD_PREFIX=/usr/local/apache2",
                "HTTPD_VERSION=2.4.58",
                "HTTPD_SHA256=fa16d72a078210a54c47dd5bef2f8b9b8a01d94909a51453956b3ec6442ea4c5",
                "HTTPD_PATCHES="
            ],
            "Cmd": [
                "httpd-foreground"
            ],
            "ArgsEscaped": true,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "/usr/local/apache2",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null,
            "StopSignal": "SIGWINCH"
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 167482044,
        "VirtualSize": 167482044,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/6695783a5c3493b4e13df145bb12550367c4ab8ba7102d354769af101e3d987e/diff:/var/lib/docker/overlay2/f04f00019a061a227864750348e7a09ffa0dd736c20590333e27a6b617629986/diff:/var/lib/docker/overlay2/a4d4607f87223c2662eb4af14b04887b5c304e617e3916c47c063fcf4850eabb/diff:/var/lib/docker/overlay2/6a070fb915edfe6af938fc86af21c3816f37f63e0a0d786d4683fbb8f693ae27/diff:/var/lib/docker/overlay2/ff28226ac6881fcefcaa6853e7d198a1752de1fc6a98ed97e9758772e3d2dbcd/diff",
                "MergedDir": "/var/lib/docker/overlay2/be02fa9672449f0c7fba9383630c831569ab04481ee33088383c0cbc4d55cb12/merged",
                "UpperDir": "/var/lib/docker/overlay2/be02fa9672449f0c7fba9383630c831569ab04481ee33088383c0cbc4d55cb12/diff",
                "WorkDir": "/var/lib/docker/overlay2/be02fa9672449f0c7fba9383630c831569ab04481ee33088383c0cbc4d55cb12/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:7292cf786aa89399bca4e3edd105d3b2ee0683a46ef1f5ff436c0f9d1d49e765",
                "sha256:1b65777f123810bd9b450a5e6505edfae1b4ffc4b326efcb66c827951893a1ef",
                "sha256:5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef",
                "sha256:87939ee964f4a95c403b7144dd470aa335042501d52cbca98eaf7ca2156657a4",
                "sha256:8c10599774e2a026e5338903f54170de38589f807613dd68665550d65a204b05",
                "sha256:8cd4026118f7e29ba95bc1c80c73f08acb8a4b9fee28b459a8e33404edcb9339"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
root@ip-172-31-16-115:~# docker image inspect httpd
```

### **Docker file*
---
- Docker can build images automatically by reading the instructions from a Dockerfile. 
- A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. 

 The Dockerfile supports the following instructions:                                 
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
| WORKDIR       | Change working directory.  
