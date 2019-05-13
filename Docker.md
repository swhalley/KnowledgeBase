# docker and compose commands

### Restart Docker
Good for development, this command stops the last running instance of docker, cleans up running containers and then starts up a fresh instance. Building new containers each time. via Cristian T 2019-05-13

`docker-compose down -v --remove-orphans && docker-compose up --build -d`

### System Information
How much disk space docker is currently using

`docker system df`
```
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              10                  3                   3.828GB             3.515GB (91%)
Containers          3                   2                   63B                 0B (0%)
Local Volumes       32                  1                   1.926GB             1.877GB (97%)
Build Cache         0                   0                   0B                  0B
```


# Out of Date
There are old pieces here (pre v12) 
need to include docker compose

# Install
Go through these steps
https://docs.docker.com/engine/installation/mac/

I found the guide here incomplete
https://docs.docker.com/mac/step_one/

Run through this guide. At time of writing,  Docker on Mac is using Docker Toolbox
After install goes, make sure to read 
https://docs.docker.com/engine/installation/mac/#from-your-shell

    docker-machine create --driver virtualbox default
    docker-machine ls
    eval "$(docker-machine env default)"
    docker run hello-world


# Building Own Image
Create a Dockerfile. This example is taken from the offical Nodejs documentation. (https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)
    
    FROM node:argon
    
    # Create app directory
    RUN mkdir -p /usr/src/app
    WORKDIR /usr/src/app
    
    # Install app dependencies
    COPY package.json /usr/src/app/
    RUN npm install
  
    # Bundle app source
    COPY . /usr/src/app
    EXPOSE 8080
    CMD [ "npm", "start" ]

Create a node app and package.json file in the same directory. The node app will be brought in as part of the build process. This really isn't ideal for development purposes. Eventually we will want to mount a directory and/or pull from github when building an image.

Next build the image. By building the image you are pulling down the argon build of node (`FROM node:argon`) which is a current long term support version as well as OS data that was chosen by the node team. We could have built this out on a lower level and picked the OS and version of node ourselves, but why bother.

Go to the directory where your docker file is hosted and run the following. This builds, lists and runs the image
    
    docker build -t swhalley/myFirstDockerImage .
    docker images
    docker run -p 49160:8080 -d swhalley/myFirstDockerImage
    docker ps

The final command, `docker ps` will list the running container and show you that the port that is exposed.
The command `-p 49160:8080` maps the internal docker port 8080 to the external port 49160 of your machine.

If your node app started a server, you can now hit the running docker container. The URL to the Docker container on mac can be found using `docker-machine ls` which will show you the installed vm's and their IP address. MAC is different from Linux installs, because there is no true native support. If using linux the IP would have been bound to 0.0.0.0 (aka localhost).

# Quick Hits
Dockerfile
`FROM Alpine`
    * Use Alpine for a small linux distort

```
docker run 
-d run in background (daemon)
-p 5432:80 <- Map 5432 inside container to 80 outside
```

`docker exec -it 9e9ff8b7870c /bin/sh`

### Delete all containers, images and volumes
`docker rm $(docker ps -a -q)`

`docker rmi $(docker images -q)`

`docker volume rm $(docker volume ls -qf dangling=true)`

`docker rm $(docker ps -a -q) && docker rmi $(docker images -q) && docker volume rm $(docker volume ls -qf dangling=true)`

# Mount Directory

# Github integration with Docker

# Frustrating Errors I ran into
### Cannot connect to the Docker daemon. Is the docker daemon running on this host?
This is caused because the docker default vm isn't running. To solve the problem run the following command
    
    docker-machine start default
