#Install
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


#Building Own Image
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
