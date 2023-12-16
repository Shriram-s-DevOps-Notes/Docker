# Docker
* To install the Docker either you can visit the below website and search for your operating system and install
```
https://docs.docker.com/engine/install/
```
You can also install docker using the below pre-written shell script on 
https://get.docker.com/ **
```
curl -fsSL https://get.docker.com -o get-docker.sh
```
Now run the script
```
sh get-docker.sh
```
## Basic-Container commands **
    
Check Our Docker Install and Config
    
```bash
docker version
#or
docker info
```
    
To run NGINX Container in foreground
    
```bash
docker container run --publish 80:80 nginx
```
    
To run NGINX Container in background
    
```bash
docker container run --publish 80:80 --detach nginx
```
    
* Different flags to run containers
    
To publish the port
    
```bash
--publish 80:80   
     #Or 
--p 80:80 
```
    
 ### What happens in  **docker container run:**
1. Looks for that image locally in image cache, doesn't find
anything
2. Then looks in remote image repository (defaults to Docker Hub)
3. Downloads the latest version (nginx:latest by default)
4. Creates new container based on that image and prepares to
start
5. Gives it a virtual IP on a private network inside docker engine
6. Opens up port 80 on host and forwards to port 80 in container
7. Starts container by using the CMD in the image Dockerfile
    
To check all running containers
    
```bash
docker container ls
#or
docker ps
```
    
To stop the containers
    
```bash
docker container stop <3-digit container id>
```
    
To check running as well as stopped container
    
```bash
docker container ls -a
    
#or
    
docker ps -a
```
    
To check the logs of the containers
    
```bash
docker container logs <container-name>
```
    
To check the process and process ID inside the container
    
```bash
docker container top <container-name>
```
    
To remove an container 
    
```bash
docker container rm <container-name>
```
    
To remove multiple containers
    
```bash
docker container rm <container-name> <container-name>
```
    
When the container is running it will not allow the users to delete the container so to remove running container run 
    
-f = Force remove
    
```bash
docker container rm -f <container-name>
 ```
