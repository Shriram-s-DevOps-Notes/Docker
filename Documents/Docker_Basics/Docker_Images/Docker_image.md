# Docker Images

- **An image is a read-only template with instructions for creating a Docker container.** Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.
### What is docker image ?
- Official definition: **"An Image is an ordered collection of root
filesystem changes and the corresponding execution
parameters for use within a container runtime."**

- Docker image consist of following things
1. App binaries and dependencies
2. Metadata about the image data and how to run the image
3. Small as one file (your app binary) like a golang static binary
4. Big as a Ubuntu distro with apt, and Apache, PHP, and more
installed

- Not a complete OS. No kernel, kernel modules (e.g. drivers)

### Difference Between Docker Containers and Docker Image:
---
- The major difference between a container and an image is the top writable layer.

- All writes to the container that adds new or modifies existing data are stored in this writable layer.

    ![Alt text](image.png)

### Docker hub

Docker Hub is a cloud-based repository service where users can create, test, store and distribute container images. It's a part of Docker's suite of cloud services for building and sharing containerized applications and microservices. Docker Hub allows developers to:

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

- To create the docker hub repo click on link **https://hub.docker.com/**

- Once the account created login to docker hub.

- You can login to docker hub through your machine terminal. To login run the command
```
docker login
```
- Add username and password.
