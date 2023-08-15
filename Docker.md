# Docker
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
ddocker run -dp 127.0.0.1:3000:3000 getting-started
```
> the `-d` flag (short for `--detach`) runs the container in the background. The `-p` flag (short for `--publish`) creates a port mapping between the host and the container. The `-p` flag takes a string value in the format of HOST:CONTAINER, where HOST is the address on the host, and CONTAINER is the port on the container.
##### list containers
```bash
docker ps
```
##### stop and remove container
```bash
docker stop <the-container-id>
docker rm <the-container-id>
docker rm -f <the-container-id> # You can stop and remove a container in a single command by adding the force flag 
```

##### push to Docker HGub
```bash
docker login -u YOUR-USER-NAME.

docker push docker/getting-started
```
##### list images
```bash
docker image ls
```
