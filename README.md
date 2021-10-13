# Docker

With Docker, **you create and manage Containers - basically packages of code and tools required to run that code**. These containers allow you to run your programs in a predictable, environment-independent way - no matter where you need to run it. For modern DevOps but also for local development - on your own or in a team - this is a winner feature since you will no longer have any "but it worked on my machine" discussions. It works inside of a container, hence it works everywhere!

- we want to have the exact same environment for development and production, this ensures that it works exactly as tested
- it should be easy to share a common development environment/setup with new people

## Containers
![img.png](../Airflow/Docker/img.png)

## Images
![img_1.png](../Airflow/Docker/img_1.png)

Containers in the end are small packages, so a container is there's a running unit of software. It is the thing which you run in the end. But when working with Docker, we also need dissolver concept called Images, because images will be the templates, the blueprints for containers, it's actually the image, which will contain the code and the required tools to execute the code, and then it's the container that then runs and executes the code.

![img.png](../Docker_Kubernetes/img.png)

## Two types of external data storages

![img_1.png](../Docker_Kubernetes/img_1.png)

### Volumes

Volumes are folders (and files) managed on your host machine which are connected to folders /
files inside of a container.
There are two types of Volumes:
- Anonymous Volumes: Created via `-v /some/path/in/container` and removed
automatically when a container is removed because of --rm added on the docker run
command
- Named Volumes: Created via -v some-name:/some/path/in/container and NOT
removed automatically

With Volumes, data can be passed into a container (if the folder on the host machine is not
empty) and it can be saved when written by a container (changes made by the container are
reflected on your host machine).

Volumes are created and managed by Docker - as a developer, you don't necessarily know
where exactly the folders are stored on your host machine. Because the data stored in there is
not meant to be viewed or edited by you - use "Bind Mounts" if you need to do that!
Instead, especially Named Volumes can help you with persisting data.

Since data is not just written in the container but also on your host machine, the data survives
even if a container is removed (because the Named Volume isn't removed in that case). Hence
you can use Named Volumes to persist container data (e.g. log files, uploaded files, database files
etc).

### Bind Mounts

Bind Mounts are very similiar to Volumes - the key difference is, that you, the developer, set the
path on your host machine that should be connected to some path inside of a Container.
You do that via `-v /absolute/path/on/your/host/machine:/some/path/inside/of/container`.

The path in front of the : (i.e. the path on your host machine, to the folder that should be shared
with the container) has to be an absolute path when using -v on the docker run command.
Bind Mounts are very useful for sharing data with a Container which might change whilst the
container is running - e.g. your source code that you want to share with the Container running
your development environment.

Don't use Bind Mounts if you just want to persist data - Named Volumes should be used for
that (exception: You want to be able to inspect the data written during development).

In general, Bind Mounts are a great tool during development - they're not meant to be used in
production (since you're container should run isolated from it's host machine)


## Key Docker Commands
```
# https://docs.docker.com/engine/reference/run/
# Build a Dockerfile and create your own Image based on the file
docker build .
# pull image, if image doesn't exist locally, it'll find it on DockerHub
docker run node
#Pull (download) an image from DockerHub
docker pull node # this is done automatically if you just docker run node and the image wasn't pulled before
# Push an image to DockerHub (or another registry) - the image name/tag must include the repository name/ url
docker push IMAGE

#check the container that are currently running
docker ps
#check the container that docker created including the stopped ones
docker ps -a

#List all locally stored images
docker images

# remove images (IMAGE ID or image name)
docker rmi 3ac38cc5cadf
# Remove all dangling images (untagged images)
docker image prune 
# Remove all locally stored images
docker image prune -a 
#stop container
docker stop compassionate_payne
# remove containers with name compassionate_payne(must stop first before remove)
docker rm compassionate_payne
# Remove all stopped containers
docker container prune 

#restart container
docker start compassionate_payne
# docker attach CONTAINER
docker attach compassionate_payne / docker start -a compassionate_payne

# run container in localhost:3000
docker run -p 3000:80 3091f996d379b9fd33a81a29dbd2f31a86ff5e97a6e6a75aada753eac834425

# you can attach yourself to a detached container 
docker run -p 3000:80 -d 3091f996d379b9fd33a81a29dbd2f31a86ff5e97a6e6a75aada753eac834425

# restart container
docker ps -a

# Create an Anonymous Volume inside a Container
docker run -v /path/in/container IMAGE :
# Create a Named Volume (named some-name ) inside a Container
docker run -v some-name:/path/in/container IMAGE
#  Create a Bind Mount and connect a local path on your host machine to some path in the Container
docker run -v /path/on/your/host/machine:path/in/container IMAGE

# List all currently active / stored Volumes (by all Containers)
docker volume ls
# Remove a Volume by it's name (or ID)
docker volume rm VOL_NAME
# Remove all unused Volumes
docker volume prune
```
