## Terms

`image`:  

`container`: live instance of an image. An image can spawn multiple containers (each one is isolated) 

## processes
 
 `docker ps`: show currently running containers
 
 `docker ps --all`: show all containers (running, stopped)
 
 ## image->container
 
 `docker create <image_name> <default_cmd>`: create a new container from image <image_name>. The container id is returned but the container is not started
 
 `docker start <container_id>`: start the container <container_id>. If the container was stopped, restart it **and run <default_cmd> again**. By default, STDOUT is not attached
 
 `docker start -a <container_id>`: start the container <container_id> and attach STDOUT (and STDERR)
 
 
 `docker run <image_name> <default_cmd>`: create a new container from image <image_name> and run <default_cmd> inside of it. By default, STDOUT and STDERR are attached to the shell, but STDIN is not
 
 `docker run -it <image_name> <default_cmd>`: create a new container from image <image_name> and run <default_cmd> inside of it, with also a nice formatted terminal to send input
 
 
 
