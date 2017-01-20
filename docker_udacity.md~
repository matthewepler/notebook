#The Complete Docker Course
[Udemy](https://www.udemy.com/docker-tutorial-for-devops-run-docker-containers/learn/v4/t/lecture/5640254)

### Images vs. Containers
**Images**
* read-only templates used to create containers
* created w/ docker `build` command
* composed of layers of other images
* sotred in registry, like Docker Hub

**Containers**
* created from images
* if an image is a class, then a container is an instance, a run-time object
* lightweight/portable encapsulation in which to run an application
* contains all binaries and dependencies needed to run the application

### Registries vs. Repositories
**Registries**
* where to store images
* host your own or use public ones like Docker Hub

**Repositories**
* In a registry, images are stored in repositories
* separate from git, but similar
* A docker repo is a collection of different docker images with the same name, but which have different tags. Each tag represents a different version. 

### Create and Run a Container
Find an image you want to run, e.g. 'busybox'
Command = `run`, format = `docker run <name>:<tag> (<cmd>) (<arg>)
`docker run busybox:1.24 
To run with a command: 
`docker run busybox:1.25 ls /` This overrides whatever the default command is for the image which is normally run at startup

 **Interactive Shell**
`-i` = interactive
`-t` = TTY for stdin/stdout
`exit` to exit the interactive prompt

[Docker Run Reference Page](https://docs.docker.com/engine/reference/run/#restart-policies-restart)

**Persistence**
Every time you start a container, it is a new instance. Any files you ay have created will be gone if you're just starting/stopping. To add the files to the image so that they exist on the next startup, you will need to build the image. **true? come back to verify**

`-d` = detached mode. Runs in background. Returns container ID.
`ps` = lists running containers and info
`ps -a` = lists all containers, even if they're not running
`--rm` removes container whn it exits

### Containers in Depth
Containers are given a random name at runtime. To specify your own name, use `--name`
`docker run --name hello_world busybox:1.24`

`docker inspect` displays low-level info on a container

### Docker Port Mapping & Docker Logs
-p <host_port>:<container_port>
`docker run -p tomcat 8888:8080`

In the above example, tomcat's default port within the container app is 8080. To view the server's responses from within the container, you would visit `localhost:8080`.

After mapping to the host's port 8888, you would point the browser of the host machine to `localhost:8888`.

### Docker Image Layers
`docker history busybox:1.24` shows layers of the image

Example Image:
```
--------------
| container  | -> writable, created at runtime with `run` command
--------------
| add Apache |
--------------
| add emacs  |
--------------
| Debian     |
------------------
| kernel, bootfs |
------------------
```

Whent the container (instance of the image) is done running, it is deleted. The underlying image remains unchanged.

Multiple containers can share access to the same underlying image.

### Building Docker Images 
* Option #1: commit changes made in a Docker container
* Option #2: write a Dockerfile

**Option 1: commit changes ina Docker container**
* spin up a container from base image
`docker run -it debian:jessie`
* do stuff to it (add files)
`apt-get update && apt-get install -y vim`
* commit changes (get id from `ps`)` 
`docker commit <container_id> mepler/debian:1.0.1`

**Option 2: write a Dockerfile**
create a file 'Dockerfile' without any '.'
`touch Dockerfile`

sample Dockerfile
```
FROM debian:jessie
RUN apt-get update && apt-get -y git
```

Each command becomes a layer. For this reason it is recommended to chain commands to reduce number of layers.

To build, use the 'build' command with `-t` to add a tag. Also include the path ('.' if you're in the directory where you're building the image and the Dockerfile is there too)
`docker build -t jameslee/debian .`

Each step in the build process is run as a container and destroyed when done. Then a new container is made for the next step, and so on. Each run command will execute on the top writable layer of the container. Docker commits that to a new image. The new image is used for the next step in the Dockerfile. 

Example of multiline RUN arguments:
```RUN apt-get install -y \
	git \
	python \
	vim 
```
Best practice is to list the arguments in alphabetical order.

The CMD command specifies what command you want to run when the container starts up. If not defined, Docker uses the default CMD written in the base image. For example, the 'debian' image's default command is `bash`.

Best practice for the CMD is to format it in the 'exec' form:
`CMD ["echo", "hello world"]`

The 'build' command relies on caching of previous builds for effecient build times. Sometimes this can break builds. For example, in this file
```
RUN apt-get update
RUN apt-get install -y vim
```
If I added `git` to the end of the second line and rebuilt the image, the first line would be skipped since it was not changed and is in the build cache. That means I risk not pulling the most up-to-date references for git when it's installed. 

In general, caching is not dangerous. But when you need to make sure you're not relying on it, use the `--no-cache` flag.











