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
