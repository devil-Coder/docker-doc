
# Getting Started with Docker

> [See this video for installation](https://www.youtube.com/watch?v=S7NVloq0EBc)

> Get stated with `docker run hello-world`

```
## List Docker CLI commands
docker
docker container --help

## Display Docker version and info
docker --version
docker version
docker info

## Execute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq

## Docker build command
docker build -t <username>/<image-tag>
```

> ## How to create your first image for your `Node.js App`

1. Create a docker file in your node.js app
    > `touch dockfile`
2. In dockfile add the following
    ```
    FROM node:carbon
    
    # Create app directory
    WORKDIR <PATH TO YOUR app.js or server.js>
    
    # Install app dependencies
    # A wildcard is used to ensure both package.json AND package-lock.json are copied
    # where available (npm@5+)
    COPY package*.json ./
    
    RUN npm install
    # If you are building your code for production
    # RUN npm install --only=production
    
    # Bundle app source
    COPY . .
    
    EXPOSE <PORT_NUMBER>
    CMD [ "npm", "start" ]
    ```
3. Next add `.dockerignore`
    > `touch .dockerignore`

4. Add the files to be ignored like node_modules, .env file or any other executables
    ```
    node_modules
    npm-debug.log
    .env
    .idea
    ```
5. Now buid your image and ship it to docker :)
    > `docker build -t <your username>/<image-tag> .`
    * Note the `.` at last.
    * Image tag helps you uniquely identfy your app on docker.
6. Check if you image is listed 
    > `docker images`
7. Run the image 
    * Running your image with -d runs the container in detached mode, leaving the container running in the background. The -p flag redirects a public port to a private port inside the container. Run the image you previously built:
         > `docker run -p <HOST_PORT_NUMBER>:<PORT_NUMBER> -d <your username>/<image-tag>`
         * For host port number see this 
            * Open Oracle VM VirtualBox Manager
            * Select the VM used by Docker
            * Click Settings -> Network
            * Adapter 1 should (default?) be "Attached to: NAT"
            * Click Advanced -> Port Forwarding
            * Add rule: Protocol TCP, Host Port 8080
         * App will be available on your docker's IP which is `192.xx.xx.xx:<PORT_NUMBER>`
         * To get the output in localhost:3000, do the following ```https://stackoverflow.com/a/36458215/7335056```
    * Print the output of your app:
       ```
        # Get container ID
        $ docker ps
        
        # Print app output
        $ docker logs <container id>
        
        # Example
        App running on port 3000
       ```
8. Check the container in which the image is running
    > `docker ps`
9. Push the image to docker hub
    > `docker push <username>/<image-tag>`
10. Check if the image is pushed or not
    * Go to hub.docker.com
    * Navigate to repositories
    * Find your image 
> Congratulations!! Your image is successfully pushed to docker hub. You can now run it on any server with docker installed.