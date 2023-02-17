# Docker

## General

- dokcer ps - list of running containers
- docker VSC

## Dockerfile

- .dockerignore file
- run in layers
- FROM - base image
- WORKDIR /app
- COPY from to - package\*.json ./
- RUN npm install
- COPY from to - . .
- ENV
- EXPOSE port
- CMD ["npm", "run"]

## Buliding image

- docker build -t name/image-name:ver path

## Commands

- docker image tag oldtag newtag - rename
- docker ps - show running containert
- docker ps -a - show all containers
- docker rm containerName
- docker images
- docker rmi imageName

## Running image

- docker run -p 3000:300 -d imageName - run port 3000 in detached
