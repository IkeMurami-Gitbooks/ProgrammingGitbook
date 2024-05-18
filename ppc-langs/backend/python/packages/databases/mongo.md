# Mongo

## PyMongo

Библиотека-клиент на Python: [https://pymongo.readthedocs.io/en/stable/](https://pymongo.readthedocs.io/en/stable/)

## Deploy

docker: [https://hub.docker.com/\_/mongo](https://hub.docker.com/\_/mongo)

docker-compose.yml:

```yaml
version: '3'

services:
  asman-mongo:
    image: mongo
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: asman
      MONGO_INITDB_ROOT_PASSWORD: password
    networks:
      - asman-network

  asman-mongo-express:
    image: mongo-express
    restart: always
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: asman
      ME_CONFIG_MONGODB_ADMINPASSWORD: password
      ME_CONFIG_MONGODB_URL: mongodb://asman:password@asman-mongo:27017/
      ME_CONFIG_BASICAUTH: false
    ports:
      - 7870:8081
    networks:
      - asman-network

networks:
  asman-network:

```
