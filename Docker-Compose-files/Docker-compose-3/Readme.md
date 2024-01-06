# Jenkins

- *After creating the docker-compose.yml file create a file in same directory where the compose file is present*

```
mkdir jenkins_home
```

- *We have to give acesss to normal users. For that run*

```
sudo chown 1000:1000 jenkins_home -R
```

- *Now run*
```
docker-compose up -d
```

