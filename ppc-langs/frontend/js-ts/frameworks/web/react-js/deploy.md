# Deploy

## Build

Запихиваем в папку deploy

Dockerfile

```docker
ARG NODE_VERSION=15.12.0-alpine3.10

FROM node:${NODE_VERSION} AS creater

WORKDIR /source

COPY . .

RUN npm i
RUN npm run build

FROM node:${NODE_VERSION} AS starter

WORKDIR /app

COPY --from=creater /source/build /app

RUN npm i -g serve

ENTRYPOINT [ "serve" ]
```

Docker compose

```yaml
version: '3'

services:
  app:
    build: 
      context: ..
      dockerfile: ./deploy/Dockerfile
    container_name: reactjs-app
    restart: always
    tty: true
    ports:
      - 9091:3000


```
