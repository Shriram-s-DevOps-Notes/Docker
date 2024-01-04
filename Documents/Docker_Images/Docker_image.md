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
