# Docker
#### Snippets
##### Getting sterted
[docs.docker.com/get-started](https://docs.docker.com/get-started/02_our_app/)
```zsh
touch Dockerfile
docker build -t getting-started .
```
```Containerfile
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```
```bash
ddocker run -dp 127.0.0.1:3000:3000 getting-started
```
```bash
docker ps
```
