# Jenkins

- *After creating the docker-compose.yml file create a file in the same directory where the compose file is present*

```
mkdir jenkins_home
```

- *We have to give access to normal users. For that run*

```
sudo chown 1000:1000 jenkins_home -R
```

- *Now run*
```
docker-compose up -d
```
- *Copy your machine IP address and add port-number [IP-Address]:[Port-Number] in the browser*
- *You will get a page like as shown below*
- *Now to get the password for the Jenkins webpage come back to your machine CLI and run the below command*
```
docker logs -f <container-name>
```
