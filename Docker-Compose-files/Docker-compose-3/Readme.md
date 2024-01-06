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

![Screenshot 2024-01-06 192028](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/8ceab827-dad0-4368-9c56-e1be50185853)

- *Now to get the password for the Jenkins webpage come back to your machine CLI and run the below command*
```
docker logs -f <container-name>
```
