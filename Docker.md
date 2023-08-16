# Docker

**Images**
> The **file system** and **configuration** of our application which are used to **create containers**.

**Containers**
> Running **instances** of Docker **images** — containers run the actual applications. A container **includes** an **application** and all of its **dependencies**. It **shares the kernel** with other containers, and runs as an **isolated process** in user space on the host OS.

**Docker Daemon** - The background **service** running on the host that **manages** **building**, **running** and **distributing** Docker **containers**.

**Docker Client**
> The **command line** tool that allows the user to interact with the Docker daemon.

**Docker Hub**
> Store is, among other things, a registry of Docker images. You can think of the registry as a directory of all available Docker images.

**Dockerfile**
> A text file that contains all the instructions to build an image.

**Volumes**
> A special Docker container **layer** that allows **data to persist** and be shared separately from the container itself. Think of volumes as a way to abstract and manage your persistent data separately from the application itself.

**Image Layers**
> A Docker **image is built** up from a series of **layers**. Each layer represents an **instruction** in the image’s **Dockerfile**. Each layer except the last one is **read-only**.

> This is a powerful concept because it means that if we need to make a change to our application, it may only affect a single layer!

Using **Cache**
> Docker recognized that we had already built some of these layers in our earlier image builds and since nothing had changed in those layers it could simply use a cached version of the layer, rather than pulling down code a second time and running those steps.
> Docker is intelligent enough to build the container in the most efficient way possible, as opposed to repeatedly building an image from the ground up each and every time.

## Docker Syntax

> Instead of using the full container ID you can use just the first few characters, as long as they are enough to uniquely ID a container.

* `image`
  * `ls`
  * `tag` \<image-id> <\tag>
  * `history` \<image-id> 
  * `inspect` \<image> returns details on the container image, the commands it runs, the OS and more
* `container`
  * `ls` or `ls -a`
  * `diff` \<container-id>

* `pull`
* `build` `--tag` or `-t` \<name>:\<tag> \<location>
* `start` \<container-id>
* `exec` execute command inside a container
  * MYSQL-CONTAINER-ID COMMAND
* `run`or `run` \<image> \<command>
  * `--detach` or `-d`
  * `--publish` or `-p`\<[host-ip:]host-port:docker-port>


## Dockerfile Syntax

[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

* `FROM` image
* `WORKDIR`
  * `WORKDIR` /path/to/workdir
  > sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD
  * combination with `ADD`, `COPY` etc.:`ADD test.txt /absoluteDir/` or * `ADD test.txt relativeDir/`
* `COPY` source destination # relative to current position and container root
  * `COPY [--chown=<user>:<group>] [--chmod=<perms>] ["<src>",... "<dest>"]`
* `RUN` verb parameter etc
  > Allows you to **install** your **application** and **packages** required for it. It executes any command in top of current image and creates **new layer** by commiting the results.
* `CMD`
  * * `CMD ["stored","as","list"]`
  > It allows you set a **default command**, which will be **executed** only **when** you **run** a **container** without specifying a command. If container **runs with a command** then the default CMD command will be **ignored**.
* `ENTRYPOINT` ["verbs","and","parameters"]
  > Similar to CMD because it allows specify a command with parameters. Only difference is that when container runs ENTRYPOINT commands and parameters are **not ignored**.
  
#### Snippets
##### Getting sterted
[docs.docker.com/get-started](https://docs.docker.com/get-started/02_our_app/)
1. ##### create a Dockerfile
```zsh
touch Dockerfile
```
```Containerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```
2. ##### build image
```bash
docker build -t IMAGE-NAME .
```
3. ##### start a new container
```bash
docker run -dp 127.0.0.1:3000:3000 IMAGE-NAME # or just docker run -dp 3000:3000 IMAGE-NAME
```
> the `-d` flag (short for `--detach`) runs the container in the background. The `-p` flag (short for `--publish`) creates a port mapping between the host and the container. The `-p` flag takes a string value in the format of HOST:CONTAINER, where HOST is the address on the host, and CONTAINER is the port on the container.
##### list containers
```bash
docker ps # process status
```
##### stop and remove container
```bash
docker stop <the-container-id>
docker rm <the-container-id>
docker rm -f <the-container-id> # You can stop and remove a container in a single command by adding the force flag 
```

##### push to Docker Gub
```bash
docker login -u <my-user-name>.

docker push docker/getting-started
```
##### list images
```bash
docker image ls
```

## Persistance

`docker volume create <name>`

### Volume and Bind mount Comparisons
The following table outlines the main differences between volume mounts and bind mounts.

 	Named volumes	Bind mounts
|Host location	| Docker chooses	| You decide|
----------------|-----------------|-----------|
|Mount example (using --mount)	| type=volume,src=my-volume,target=/usr/local/data	| type=bind,src=/path/to/data,target=/usr/local/data|
|Populates new volume with container contents	| Yes |	No|
|Supports Volume Drivers |	Yes	| No|


1. Mount your source code into the container
1. Install all dependencies
1. Start nodemon to watch for filesystem changes

##### view logs
```bash
docker logs -f <container-id>
```

### Multi Container apps
Up to this point, you’ve been working with single container apps. But, now you will add MySQL to the application stack. The following question often arises - “Where will MySQL run? Install it in the same container or run it separately?” In general, 

> each container should do one thing and do it well.

The following are a few reasons to run the container separately:

* There’s a good chance you’d have to **scale** APIs and front-ends **differently** than **databases**.
* Separate containers let you version and **update versions** in isolation.
* While you may use a container for the database locally, you may want to use a managed service for the database in production. You don’t want to ship 
  your database engine with your app then.
* Running **multiple processes** will require a **process manager** (the container only starts one process), which adds complexity to container startup/shutdown.

> If you place the two containers on the same network, they can talk to each other.

* Assign the network when starting the container.
* Connect an already running container to a network.

##### Assign the network (when starting the container)
```bash
docker network create todo-app
```
# Material Review

* How can I create a new container image?
  > Manually installing software in a container and then committing.
  > Using Dockerfile.
  > An image is basically a diff.
* What is a parent image and how can I decide on the right one?
  > A parent image is the image that your image is based on. It refers to the contents of the FROM directive in the Dockerfile.
  > search on Docker Store / Docker Hub, as well as Docker Trusted Registries that you might run in your own environmen.
* How can I copy files to an image?
  > `COPY` keyword in Dockerfile, ...
* How can I run commands during the building process?
  > `RUN` in Dockerfile, ...
* How can I specify what commands should be ran when a container starts up?
  > `CMD` or `ENTRYPOINT` in Dockerfile, ...
* What does it mean that image files have a layered structure, and what consequences does it have?
  > Each layer is an image itself, just one without a human-assigned tag. They have auto-generated IDs though.
  > Each layer stores the changes compared to the image it’s based on.
  > Each instruction in a Dockerfile results in a layer. (Except for multi-stage builds, where usually only the layers in the final image are pushed, or when an image is squashed to a single layer).
  > Layers are used to avoid transferring redundant information and skip build steps which have not changed (according to the Docker cache).
* What is the difference between the RUN and CMD commands?
  > *see above* [Dockerfile syntax](#Dockerfile-syntax)
* What are Docker tags and what are they used for?
  > A tag points to a single Docker image.
* How can I run a container, in a way that it's immediately deleted after it's stopped running?
  > `docker run --rm IMAGE-NAME` [powercms.com](https://www.powercms.in/article/how-automatically-delete-docker-container-after-running-it)
* How to Deploy a Docker container to Heroku?
  > Heroku is a popular platform with a great pipeline for deploying sites.
