# Docker
Why we using docker

- low impact on OS, work really fast
- can share, rebuild and distribute very easily (it should be easy to share a common development environment/setup with new people)
- we can have the exact same environment for development and production, this ensures that it works exactly as tested

## Containers
Containers are running instances of Images. When you create a container (via `docker run` ), a thin read-write layer is added on top of the Image. 

Multiple Containers can therefore be started based on one and the same Image. All Containers run in isolation, i.e. they don't share any application state or written data. You need to create and start a Container to start the application which is inside of a Container. So it's containers which are in the end executed - both in development and production.

## Images

![img_3.png](img_3.png)

- **images will be the templates, the blueprints for containers, it's actually the image, which will contain the code and the required tools to execute the code, and then it's the container that then runs and executes the code.**
- Images are either pre-built (e.g. official Images you find on DockerHub) or you build your own Images by defining a `Dockerfile`.
    - `Dockerfiles` contain **instructions** which are executed when an image is built ( `docker build .`)
- **Images are locked and finished once you built them**. **Everything in the images is read only then,** and you can't edit it from the outside by simply updating your code. The image doesn't care about the past. Once this copy operation is done, you can change your outside code however you want. You can even delete it and the image will not be affected. You need to rebuild to pick up external changes and basically copy all the updated code into the image. So therefore, what we need to do here is we need to run `docker build .` again to rebuild this image and therefore to build a new image in the end.

## Two types of external data storages

### Volumes - managed by docker

**Volumes are folders (and files) managed on your host machine which are connected to folders / files inside of a container.** There are two types of Volumes:

- Anonymous Volumes
    - created specifically for a single container
    - survives when container shutdown/restart unless it is removed (`—rm` is used)
    - cannot share across containers
    - since it is anonymous. it cannot be reused
- Named Volumes:
    - we cannot create named volumes inside a docker file, not attached to any specific container
    - can share across containers
    - survive even we remove the container
    
    Since data is not just written in the container but also on your host machine, the data survives even if a container is removed (because the Named Volume isn't removed in that case). Hence you can use Named Volumes to persist container data (e.g. log files, uploaded files, database files etc).
    

### Bind Mounts - managed by you

The developer, set the path on the host machine that should be connected to some path inside of a Container.

The path in front of the : (i.e. the path on your host machine, to the folder that should be shared with the container) has to be an **absolute path** when using -v on the docker run command. 

Bind Mounts are very useful for sharing data with a Container which might change whilst the container is running - e.g. your source code that you want to share with the Container running your development environment.

Don't use Bind Mounts if you just want to persist data - Named Volumes should be used for that (exception: You want to be able to inspect the data written during development).

In general, Bind Mounts are a great tool during development - they're not meant to be used in production (since you're container should run isolated from it's host machine)

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
