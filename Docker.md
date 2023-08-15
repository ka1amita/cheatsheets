# Docker
## docker syntax
* `exec` execute command inside a container
  * MYSQL-CONTAINER-ID COMMAND
* `run`
  * `--detach` or `-d`
  * `--publish` or `-p`

## Dockerfile syntax

* `FROM` image
* `WORKDIR` directory
* `COPY` source destination # relative to current position and container root
* `RUN` verb parameter etc
  > Allows you to **install** your **application** and **packages** required for it. It executes any command in top of current image and creates **new layer** by commiting the results.
* `CMD` ["stored","as","list"]
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
docker build -t getting-started .
```
3. ##### start a new container
```bash
docker run -dp 127.0.0.1:3000:3000 getting-started # or just docker run -dp 3000:3000 getting-started
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

