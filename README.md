# Docker OCS inventory

## DOCKER COMMAND 

docker pull ocsinventory/ocsinventory-docker-image:2.12.1

```sh
docker run \
-p [HOST_HTTP_PORT]:80 \
-p [HOST_HTTPS_PORT]:443 \
--name [PUT_A_NAME_HERE] \
-e OCS_DB_NAME=[DB_NAME] \
-e OCS_DB_SERVER=[DB_HOST] \
-e OCS_DB_PORT=[DB_PORT] \
-e OCS_DB_USER=[DB_USER] \
-e OCS_DB_PASS=[DB_PASS] \
-itd \
ocsinventory/ocsinventory-docker-image:MY_TAG
```

## DOCKER COMPOSE FILE 

```yml
services:

  ocsinventory:
    image: ocsinventory/ocsinventory-docker-image:MY_TAG
    container_name: [PUT_A_NAME_HERE]
    ports:
      - "[HOST_HTTP_PORT]:80"
      - "[HOST_HTTPS_PORT]:443"
    environment:
      OCS_DB_NAME: [DB_NAME]
      OCS_DB_SERVER: [DB_HOST]
      OCS_DB_PORT: [DB_PORT]
      OCS_DB_USER: [DB_USER]
      OCS_DB_PASS: [DB_PASS]
    restart: unless-stopped
```
