## Basic-Container commands **
    
Check Our Docker Install and Config
    
```bash
docker version
#or
docker info
```
    
To run NGINX Container in the foreground
    
```bash
docker container run --publish 80:80 nginx
```
    
To run NGINX Container in the background
    
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
1. Looks for that image locally in the image cache, doesn't find
anything
2. Then look in the remote image repository (defaults to Docker Hub)
3. Downloads the latest version (nginx: latest by default)
4. Create a new container based on that image and prepare to
start
5. Gives it a virtual IP on a private network inside the docker-engine
6. Opens up port 80 on a host and forwards to port 80 in the container
7. Start the container by using the CMD in the image Dockerfile
    
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
    
To remove a container 
    
```bash
docker container rm <container-name>
```
    
To remove multiple containers
    
```bash
docker container rm <container-name> <container-name>
```
    
When the container is running it will not allow the users to delete the container so to remove the running container run 
    
-f = Force remove
    
```bash
docker container rm -f <container-name>
 ```
