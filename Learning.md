# Docker



###### Info
Registry:                                       <- From/To where you can download/upload images
store.docker.com


###### **Nommenclature**:
pull = fetches images from Docker Registry

###### Terminology
In the last section, you saw a lot of Docker-specific jargon which might be confusing to some. So before you go further, let’s clarify some terminology that is used frequently in the Docker ecosystem.

**Images** - The file system and configuration of our application which are used to create containers. To find out more about a Docker image, run docker image inspect alpine. In the demo above, you used the docker image pull command to download the alpine image. When you executed the command docker container run hello-world, it also did a docker image pull behind the scenes to download the hello-world image.                                     
**Containers** - Running instances of Docker images — containers run the actual applications. A container includes an application and all of its dependencies. It shares the kernel with other containers, and runs as an isolated process in user space on the host OS. You created a container using docker run which you did using the alpine image that you downloaded. A list of running containers can be seen using the docker container ls command.                        
**Docker daemon** - The background service running on the host that manages building, running and distributing Docker containers.         
**Docker client** - The command line tool that allows the user to interact with the Docker daemon.               
**Docker Store** - Store is, among other things, a registry of Docker images. You can think of the registry as a directory of all available Docker images. You’ll be using this later in this tutorial.           




###### Commands:

#docker image pull alpine
#docker image ls
#docker container run alpine ls -l
#docker container run -it alpine /bin/sh       <- "-it" = interactive + TTY - opens a terminal to type
#docker container ls
#docker container ls -a                        <- Shows all containers, including terminated ones
#docker container run --help                   <- To find out more about commands
#docker container start <container ID>         <- Starts a terminated container (# docker container ls -a) from it's ID
#docker container exec <container ID> ls       <- Execute certain command on a container with specific ID
#docker image inspect <IMAGE NAME>             <- Getting all info from certain image
