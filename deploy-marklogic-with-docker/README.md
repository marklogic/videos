Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved.

# Deploy MarkLogic with Docker

MarkLogic Server is a multi-model database that has both NoSQL and trusted enterprise data management capabilities. It is the most secure multi-model database, and it’s deployable in any environment.  This video introduces deploying MarkLogic in a Docker container.  


Readme: https://github.com/marklogic/marklogic-docker/blob/master/README.md 

MarkLogic Server image on DockerHub: https://hub.docker.com/r/progressofficial/marklogic-db

Docker Desktop: http://docker.com 

Rancher Desktop: https://rancherdesktop.io/ 

#marklogic 

## Install Docker Desktop or Rancher Desktop
To deploy a MarkLogic container, download [Docker Desktop](https://www.docker.com/products/docker-desktop/) or an open-source alternative such as [Rancher Desktop](https://rancherdesktop.io/).

## Start Docker or Rancher Desktop
After installing Docker or Rancher Desktop, start the application.

## Get the official Docker image
`docker pull progressofficial/marklogic-db`
## Run a MarkLogic database server in a container 
In the example, replace these values:

* `admin` - Your existing or new MarkLogic username.
* `Areally!PowerfulPassword1337` - Your existing or new MarkLogic password.
* `port` - The MarkLogic port for your app server

### (MacOS/Linux)
```
docker run -d -it -p 8000:8000 -p 8001:8001 -p 8002:8002 \ 
     -e MARKLOGIC_INIT=true \
     -e MARKLOGIC_ADMIN_USERNAME='admin' \
     -e MARKLOGIC_ADMIN_PASSWORD='Areally!PowerfulPassword1337' \
     progressofficial/marklogic-db
```
### (Windows PowerShell)
```
docker run -d -it -p 8000:8000 -p 8001:8001 -p 8002:8002 `
     -e MARKLOGIC_INIT=true `
     -e MARKLOGIC_ADMIN_USERNAME='admin' `
     -e MARKLOGIC_ADMIN_PASSWORD='Areally!PowerfulPassword1337' `
     progressofficial/marklogic-db
```

## Get the logs for a container
`docker logs [hash of container]`

## View the running MarkLogic Docker container (and any other running containers)
`docker ps`

## Stop a MarkLogic container
`docker stop [hash of container]`

## Start a MarkLogic container
`docker start [hash of container]`
