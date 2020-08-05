# Docker-CLI

## Terms

`image`:  a FS with a run command

`container`: live instance of an image. An image can spawn multiple containers (each one is isolated) 

`port`: unix port (like 8000)

`volume`: a persistent folder structure in the container

## system
`docker system prune`: remove images and container dangling

`docker system prune -a`: remove everything
 
`docker ps`: show currently running containers
 
`docker ps --all`: show all containers (running, stopped)
 
`docker images -a`: list all images 
 
 ## create image
 `docker build . -t user/image_name:version <image_name>`: create a new image from this folder (will look for Dockerfile) and tag it. If `version` is omitted latest is default.
 
`docker build . -f <file>`: create a new image from this folder using <file> as Dockerfile.
 
`docker commit -c 'CMD ["command"]' <container_id>`: create a new image from the running container <container_id>. This shouldn't really be used.
 
 ## run containers (I)
 
 `docker create <image_name> <default_cmd>`: create a new container from image <image_name> which will run <default_cmd> once started. The container id is returned but the container is not started
 
 `docker start <container_id>`: start the container <container_id>. If the container was stopped, restart it **and run <default_cmd> again**. By default, STDOUT is not attached
 
 `docker start -ai <container_id>`: start the container <container_id> and attach STDOUT, STDERR and STDIN
  
 
 ## run containers (II)
 
 `docker run <image_name/id> <default_cmd>`: create a new container from image <image_name> and run <default_cmd> inside of it. By default, STDOUT and STDERR are attached to the shell, but STDIN is not
 
 `docker run -d <image_name/id> <default_cmd>`: create a new container from image <image_name> and run <default_cmd> inside of it in detached mode (prompt is returned)
 
 
 `docker run -it <image_name/id> <default_cmd>`: create a new container from image <image_name> and run <default_cmd> inside of it, with also a nice formatted terminal to send input
 
 ## ports mapping
 Ports mapping is a **run** option, which means ports must be specified when using `docker run`. Alternatively, ports can be specified in the `docker-compose.yaml` file.
 
 `docker run -p <host-port>:<container-port>` bind input ports. Output ports are opened by default (otherwise docker container wouldn't be able to connect to the internet)
 
 
 
 ## volumes mapping
 
 ## container lifecycle
 
 `docker exec -it <container_id> <cmd>`: run <cmd> in the running container with id <container_id>. Useful to open a shell on the container 
 
 `docker stop <container_id>`: send a SIGTERM to the container. If the container ignores it, send a SIGKILL after 10s
 
 `docker kill <container_id>`: send a SIGKILL to the container.
 
 # Dockerfile
 
 
 # docker-compose
 
`docker-compose up`: start containers.
 
`docker-compose up --build`: re-build and start containers.

`docker-compose down`: stop containers.

`docker-compose ps`: list processes associated with containers defined in the `docker-compose.yml`

