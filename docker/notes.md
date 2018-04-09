# Docker
 ### Deploying first docker container-
 - `docker search <image>`
 - `docker run -d <image:tag>`
 - `docker inspect <friendly-name|container-id>`
 - `docker logs <friendly-name|container-id>`
 - `docker run -d --name <friendly-name> -p <host-port>:<container-port> <image>`
    - since container is sandboxed hence things outside the container which needs to connect with it need a port exposed. 
    - By default the port on host is mapped to 0.0.0.0 which means to all ip address, we can also define a particular IP address like `-p 127.0.0.1:6379:6379`.
      
 - `docker run -d --name redisDynamic -p <container-port> <image>`
      - which specified port we can run only one instance but this way host port will be dynamically assigned.
 - `docker port <friendlyname|container-id> <container-port>` - to find the dynamic port of host assigned
 - `docker run -d --name redisMapped -v <host-directory>:<container-directory> <image>`
      - since docker is stateless so in case of db we can map/store/mount data stored by redis in /data directory to host directory
      - docker allows us to use $PWD as placeholder to current directory.
      
 - `docker run ubuntu ps` and  `docker run -it ubuntu bash`
      - some images lets us override the command used to run the image.
    

 ### Deploy static HTML website as container (creating Dockerfile)
   - `COPY . /usr/share/nginx/html` - copy content of current dir into particular location inside container.
   - `docker build -t <imagename:tag> <build-directory>` - build-directory is usually directory of Dockerfile.
   - When a container launches, it's sandboxed from other processes and networks on the host
   - When starting a container we need to give it permission and access to what it requires.
     - For example, to open and bind to a network port on the host you need to provide the parameter `-p <host-port>:<container-port>`
     
   - `docker run -d --name mywebserver -p 80:80 <imagename:tag>` - since its web server hence port 80 is bound.


 ### Docker - Building container images
   - `RUN <command>` - allows us to execute any command as we would at a command prompt.
     - example installing different application packages or running a build command.
     - results of RUN are persisted  to image so important to remove unnecessary/temp files.     
   - `COPY <src> <dest>` - allows copy files from directory containing Dockerfile to the container's image.
     - example source code and assets that we want to be deployed inside the container.
     - If we're copying a file into a directory then you need to specify the filename as part of the destination
   - With our files copied into our image and any dependencies downloaded, you need to define which port application needs to be accessible on.
   - `EXPOSE <port>` - using this we can tell docker which port should be open and can be bound too.
     - we can define multiple ports on a single command like `EXPOSE 80 443` OR `EXPOSE 6000-7000`
   - we now need to define the command that launches the application.
   - **CMD** - cmd line in the Dockerfile defines the default command to run when a container is launched
     - if command requires argument then we need to use array e.g. `["cmd","-a","arga value","-b","argb value"]`
     - e.g. `CMD ["nginx", "-g", "daemon off;"]`
   - an alternative to **CMD is ENTRYPOINT**, while a *CMD can be overridden while container starts*, an *ENTRYPOINT defines a command which can have arguments passed to it when container launches*.
   - `docker build` - takes the directory cotaining the Dockerfile executes steps and store image in local docker engine.


 ### Dockerizing node.js application - 
   - `WORKDIR <directory>` - can define working directory to ensure all future commands are executed from directory relative to this directory(our application).
   - If you don't want to use the cache as part of the build then set the option --no-cache=true as part of the docker build command.
   - Splitting the installation of the dependencies and copying out source code enables us to use the cache when required. i.e. say copy package.json and then npm install before copying actual code.
   - If we copied our code before running npm install then it would run every time as our code would have changed. 
   - By copying just package.json we can be sure that the cache is invalidated only when our package contents have changed.
   - `COPY . <dest dir>` to copy entire directory where our Dockerfile exists.
   - using -e while running container can be use to set environment variables *-e NODE_ENV=production* e.g. `docker run -d --name my-production-app -e NODE_ENV=production -p 3000:3000 my-nodejs-app`
   - code block sample below - 
   ```
   FROM node:7-alpine
   RUN mkdir -p /src/app
   WORKDIR /src/app
   COPY package.json /src/app/package.json
   RUN npm install
   COPY . /src/app
   EXPOSE 80 443 3000
   CMD ["npm","start"]
   ```
   

 ### Docker - Optimising Dockerfile with OnBuild
   - the Dockerfile with command prefixed with **ONBUILD** result that we can build this image but the application specific commands won't be executed until the built image is used as a base image. They'll then be executed as part of the base image's build.
   - things like setting NODE_ENV or copying code from current dir to specified target directory can be done there like `ONBUILD ENV NODE_ENV $NODE_ENV` or `ONBUILD COPY package.json /usr/src/app/` or 
   `ONBUILD RUN npm install && npm cache clean --force`


### Docker - Communicating between containers
  - 
   


   
   
      
    
    