# Docker



## Info
* Registry (store.docker.com)
   * From/To where you can download/upload images.

* Dockerfile
   * Create a docker with configuration extistent inside this file. Flow:
     * Specify a base image.
     * Run some commands to install additional programs.
      * Specify a command to run container startup
        ```
        FROM alpine:1
        RUN npm install
        CMD ["npm", "start"]
        ```
        
* Docker Compose
    * Docker compose is to essentially function as a Docker CLI but allow you to kind of issue multiple commands much more quickly.
      * Essentially it works like this:
        * Docker-compose.yml
          * redis-server
            * Make it using the "redis" image
          * node-app
            * Make it using the Dockerfile in the current directory
            * Map port 8081 to 8081
            ```
            version: '3'
            services:
              redis-server:
                image: 'redis'
              node-app:
                build: .
                ports:
                  - "4001:8081"
            ```

* Swarm Mode
   * Tells Docker that you will be running many Docker engines and you want to coordinate operations across all of them. Swarm mode combines the ability to not only define the application architecture, like Compose, but to define and maintain high availability levels, scaling, load balancing, and more. With all this functionality, Swarm mode is used more often in production environments than it’s more simplistic cousin, Compose.
* Swarm:
    * Stacks: Group of interrelated services and dependencies. Orchestrated as a unit. Production applications are one stack, and sometime more.            
    * Tasks: Atomic unit of a service and scheduling Docker. One container instance per task.            
    * Service: A stack component, including a container image, number of replicas (tasks), ports and update policy.          



## Nommenclature:
* pull = fetches images from Docker Registry
* run = Run a command on a new container
* image = Docker images (like AMI)
* alpine = Basically means a small version of something (e.g.: FROM node:alpine - It is a small version of node image)


## Terminology
In the last section, you saw a lot of Docker-specific jargon which might be confusing to some. So before you go further, let’s clarify some terminology that is used frequently in the Docker ecosystem.

**Images** - The file system and configuration of our application which are used to create containers. To find out more about a Docker image, run docker image inspect alpine. In the demo above, you used the docker image pull command to download the alpine image. When you executed the command docker container run hello-world, it also did a docker image pull behind the scenes to download the hello-world image.                                     
**Containers** - Running instances of Docker images — containers run the actual applications. A container includes an application and all of its dependencies. It shares the kernel with other containers, and runs as an isolated process in user space on the host OS. You created a container using docker run which you did using the alpine image that you downloaded. A list of running containers can be seen using the docker container ls command.                        
**Docker daemon** - The background service running on the host that manages building, running and distributing Docker containers.         
**Docker client** - The command line tool that allows the user to interact with the Docker daemon.               
**Docker Store** - Store is, among other things, a registry of Docker images. You can think of the registry as a directory of all available Docker images. You’ll be using this later in this tutorial.          
**Layers** - A Docker image is built up from a series of layers. Each layer represents an instruction in the image’s Dockerfile. Each layer except the last one is read-only.               
**Dockerfile** - A text file that contains all the commands, in order, needed to build a given image. The Dockerfile reference page lists the various commands and format details for Dockerfiles.
  * Steps:
    * We essentially take the image generated on the previous process (e.g.: FROM alpine:3.7).
    * Create a new container out of it.
    * Execute a command in the container or make change to its system (e.g.: RUN apk add --no-cache mysql-client).
    * After running this command, docker takes a snapshot of its file system and save as an output (an image) for the next instruction along the chain.
    * If there is no more instruction to execute the image that was generated during the last step is output from the entire process as the final image that we really care about (e.g.: CMD ["redis-server"]).
**Volumes** - A special Docker container layer that allows data to persist and be shared separately from the container itself. Think of volumes as a way to abstract and manage your persistent data separately from the application itself.




## Commands:

* docker run = docker create + docker start
* docker image pull alpine   
* docker image ls      
* docker container run alpine ls -l 
* docker container run -it alpine /bin/sh              <- "-it" = interactive + TTY - opens a terminal to type          
* docker container ls          
* docker container ls -a                               <- Shows all containers, including terminated ones          
* docker container run --help                          <- To find out more about commands           
* docker container start <container ID> <NAME>         <- Starts a terminated container (# docker container ls -a) from its ID           
* docker container exec <container ID> ls              <- Execute certain command on a container with specific ID       
* docker image inspect <IMAGE NAME>                    <- Getting all info from certain image          
* docker container commit <container ID>               <- Create an image of one container, which can be a terminated container as well   
* docker image tag <IMAGE_ID> <NAME>                   <- Create a tag (name) to our image.         
* docker image build -t <TAG> .                        <- Create an image from a Dockerfile ".".       
* docker image inspect --format "{{ json .RootFS.Layers }}" alpine        <- get certain information from image downloaded         
* docker swarm init --advertise-addr $(hostname -i) --advertise-addr 127.0.0.1             <- Create a docker swarm "master"      
* docker swarm join-token manager                      <- This command shows instructions to add a node to this docker swarm "master"     
* docker node ls                                       <- Run this command on docker swarm leader to check all nodes connected     
* docker system prune                                  <- Remove all stopped containers, dangling images, build cache and networks not used by at least one container.
* docker stop                                          <- You send a SIGTERM to the container process to stop it. The container still have time to save things inside of it before shutting down - This command only gives docker 10 seconds (default time, can be changed using "-t") to stop things, if not, it will then run the command "docker kill" automatically, which will destroy instantly the container.
* docker kill                                          <- Issues a SIGKILL signal and will strraight forward kill the container process once and for all.
* docker pause                                         <- Issues a SIGSTOP signal that suspendes all processes. In linux is uses `cgroups freezer` to make the process unaware, and unable to capture, that it is being suspended, and subsequently resumed 
* docker exec -it <container ID> <command>             <- This command allows you to run a seccond command inside the container. "-it" (interactive + tty (allocate a pseudo-TTY)
      STDIN - Anything that you type in your console that will be sent as a command or parameter to the container.
      STDOUT - Anything that you see coming out as a result of a command that is not an error.
      STDERR - Any error that outputs out of a running process on the screen.
* CTRL + D                                             <- Logs out of the container you are connected to.
* docker exec -it <container ID> bash                  <- Is a good way to connect to a container and run bash shell to type any command after that.
* docker run -it busybox sh                            <- Is a quick and good way to spin up a container and just run any command as test or anything similar.
* docker build -t <dockerID>/<project_name> .                                       <- This command will build a container with all sets of commands inside a "Dockerfile" on the current directory
* docker run -p <external_port>:<port_inside_container> <image_id>
* docker-compose up                                 <- Start docker-compose file containers
* docker-compose up -d                              <- Start docker-compose file containers and run them on background (detach)
* docker-compose up --build                         <- Start docker-compose file containers and force to rebuild the image
* docker-compose down                               <- Stop docker-compose file containers
* docker run -v $(pwd):/app 192688e06095            <- It will basically run the container mapping the "/app" folder inside the container to the current directory on the local machine
* docker run -v /app/node_modules -v $(pwd):/app 192688e06095                       <- It will do the same as the command above, but the folder "/app/node_modules" will be picked up from inside the container, not from the local machine as the rest of the command shows.

                                                                                



## Dockerfile
To add comments, do a normal "# Comments"
* **FROM** alpine:1
* **RUN** npm install
* **CMD** ["npm", "start"]
