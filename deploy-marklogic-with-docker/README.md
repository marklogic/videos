Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved.

MarkLogic Server is a multi-model database that has both NoSQL and trusted enterprise data management capabilities. It is the most secure multi-model database, and it’s deployable in any environment.  This video introduces deploying MarkLogic in a Docker container.  

Getting Started: https://github.com/marklogic/marklogic-docker⁠

Readme: https://github.com/marklogic/marklogic-docker/blob/master/README.md 

MarkLogic Server image on DockerHub: https://hub.docker.com/r/progressofficial/marklogic-db

Docker Desktop: http://docker.com 

Rancher Desktop: https://rancherdesktop.io/ 

#marklogic 

# Deploy MarkLogic with Docker

## Get the official Docker image
`docker pull progressofficial/marklogic-db`
## Run a MarkLogic database server in a container
```
docker run -d -it -p 8000:8000 -p 8001:8001 -p 8002:8002 \ 
     -e MARKLOGIC_INIT=true \
     -e MARKLOGIC_ADMIN_USERNAME='admin' \
     -e MARKLOGIC_ADMIN_PASSWORD='Areally!PowerfulPassword1337' \
     progressofficial/marklogic-db
```
### Explanation of commands
* `-d -it`: Runs detached (background) in interactive mode with TTY

* `-p 8000:8000 -p 8001:8001 -p 8002:8002`: Maps three ports from container to host:
  * 8000: Application Services/Query Console
  * 8001: Admin Interface
  * 8002: Manage API

  *Adjust the ports as needed for your environment*

* `-e MARKLOGIC_INIT=true`: Initializes MarkLogic 
* `-e MARKLOGIC_ADMIN_USERNAME='[admin]'`: Sets admin username to "admin". Replace with your username.
* `-e MARKLOGIC_ADMIN_PASSWORD='[Areally!PowerfulPassword1337]'`: Sets the admin password. Replace with your password.
* `progressofficial/marklogic-db`: Uses the Progress Software official MarkLogic image

## Get the logs for a container
`docker logs [hash of container]`

## View the running MarkLogic Docker container (and any other running containers)
`docker ps`

## Stop a MarkLogic container
`docker stop [hash of container]`

## Start a MarkLogic container
`docker start [hash of container]`