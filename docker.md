## Terms

`image`:  a FS with a run command

`container`: live instance of an image. An image can spawn multiple containers (each one is isolated) 

## system
`docker system prune`: remove images and container dangling

`docker system prune -a`: remove everything
 
`docker ps`: show currently running containers
 
`docker ps --all`: show all containers (running, stopped)
 
`docker images -a`: list all images 
 
 ## image
 `docker build . -t user/image_name:version <image_name>`: create a new image from this folder (will look for Dockerfile) and tag it. If `version` is omitted latest is default.
 
`docker commit -c 'CMD ["command"]' <container_id>`: create a new image from the running container <container_id>. This shouldn't really be used.
 
 ## image->container
 
 `docker create <image_name> <default_cmd>`: create a new container from image <image_name> which will run <default_cmd> once started. The container id is returned but the container is not started
 
 `docker start <container_id>`: start the container <container_id>. If the container was stopped, restart it **and run <default_cmd> again**. By default, STDOUT is not attached
 
 `docker start -a <container_id>`: start the container <container_id> and attach STDOUT (and STDERR)
  
 
 `docker run <image_name/id> <default_cmd>`: create a new container from image <image_name> and run <default_cmd> inside of it. By default, STDOUT and STDERR are attached to the shell, but STDIN is not
 
 `docker run -it <image_name/id> <default_cmd>`: create a new container from image <image_name> and run <default_cmd> inside of it, with also a nice formatted terminal to send input
 
 ## run in container
 
 `docker exec -it <container_id> <cmd>`: run <cmd> in the running container with id <container_id>. Useful to open a shell on the container 
 
 
