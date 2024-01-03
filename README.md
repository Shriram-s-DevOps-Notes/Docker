# Docker
* To install Docker you can visit the below website search for your operating system and install
```
https://docs.docker.com/engine/install/
```
You can also install docker using the below pre-written shell script on 
https://get.docker.com/ 
```
curl -fsSL https://get.docker.com -o get-docker.sh
```
Now run the script
```
sh get-docker.sh
```
- For detailed information about installation click on the below link
```
https://docs.docker.com/engine/install/
---
- A container is the standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.
- Containers are OS-level virtualization technology that is used to package applications and their dependencies and run them in isolated environments.
- They provide a lightweight method of packaging and deploying applications in a standardized way across many different types of infrastructure.

### BENEFITS OF CONTAINERIZATION.
---
- Containerization brings many benefits,
    - Portability between different platforms and clouds.
    - Efficiency through using far fewer resources than virtual machines.
    - Delivering higher utilization of compute resources.
- CONTAINERIZATION TOOLS:
1. **Docker:**
-  Developed by Docker, inc.
- open-source container-orchestration system
- First started in 2013.
- **Written in Go.**
- **Docker Inc. was founded by Kamel Founadi, Solomon Hykes, and Sebastien Pahl**

2. 


## What is Docker?

- Docker is a set of the platform as a service (PAAS) products that use OS-level virtualization to deliver software in packages called containers.
- Docker is a container engine that uses the Linux kernel features like namespaces and control groups to create containers on top of the operating system and automates application deployment on the container.
- Docker is a tool designed to make it easier to create, deploy, and run applications by using containers.
- Docker containers allow a developer to package up an application with all the parts it needs, such as libraries and other dependencies, and deploy it as one single package.

 ### VM vs DOCKER
 ---
 **VM**
- The first thing you should know is, that docker containers are not virtual machines.
Understanding virtual machines:


![Alt text](image-1.png)

- It all begins with some type of infrastructure.
- On top of that host, runs an operating system.
- Then we have a thing called a hypervisor.
- The next layer in this are your guest operating system.
- Then on top of that, each gues operating system needs its own copy of various binaries and libraries.
- Finally, we have our application.

**DOCKER CONTAINERS**

![Alt text](image-2.png)

- Docker containers also runs on some type of infrastructure.
- Then, we have the host operating system.
- Then we have the daemon that replaces the hypervisor.
- Next we have the binaries and libraries.
- Then finally we have the applications running on the host operating system.
