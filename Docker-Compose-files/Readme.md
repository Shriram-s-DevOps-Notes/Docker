# Docker Compose Collections

This repository contains multiple Docker Compose files, each tailored for specific services. Below are the descriptions and instructions for each.

## Compose Files Overview

### Docker-compose-1
---
- **Webserver (Nginx) Setup**
  - **Folder**: `Webserver`
  - **Description**: This setup includes a basic Nginx server, configured to serve a static HTML page. It maps the container's port 80 to port 8080 on the host.
  - **Components**: Nginx, Custom HTML page.


### Docker-compose-2
---
- **Multiple Webpages Nginx Setup**
  - **Folder**: `MultiWebNginx`
  - **Description**: Configures Nginx to serve multiple websites, each accessible from different paths. This setup demonstrates how to host several sites from a single Nginx instance.
  - **Components**: Nginx, Site1, Site2

### Docker-compose-3
---
- **Jenkins CI/CD Setup**
  - **Folder**: `Jenkins`
  - **Description**: A Jenkins Continuous Integration and Continuous Deployment setup, allowing for automation of your development workflows. Jenkins data is persisted and available even after the container is stopped or removed.
  - **Components**: Jenkins

## General Instructions

- To use any of these Docker Compose setups, navigate to the respective folder and run:

```bash
docker-compose up -d
```
- To stop the services, you can use:
```
docker-compose down
```

### Contributing
**Feel free to fork this repository and submit pull requests to contribute to or improve the configurations.**

