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

 ### Deploy static HTML website as container
   - first
   - second
   - third
   
      
    
    