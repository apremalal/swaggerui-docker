# Swagger-UI hosted in Docker

This is cloned from repo: https://github.com/schickling/dockerfiles/tree/master/swagger-ui and existing Docker image: https://hub.docker.com/r/schickling/swagger-ui/

Swagger UI 2.2.8 as small Docker image

## Usage

This image allows many variables to be set when running swagger ui, like API URL, API KEY, OAUTH etc. (list and description below)

### Sample docker run command

```sh
$ docker run -d -p 80:80 -e API_URL=http://localhost:4000/swagger lukastosic/swaggerui-docker
```

Follow Docker manual for docker related parameters. In this example docker will run image, and map internal container port 80 to host port 80

### Variables

* `API_URL` - Swagger endpoint for your API
* `API_KEY` - Default API Key (optional)
* `PORT` - Default port to run on (optional)
* `VALIDATOR_URL` - Swagger validator (optional)
* `OAUTH_CLIENT_ID` - oAuth2 client ID (optional, use empty to remove)
* `OAUTH_CLIENT_SECRET` - oAuth2 client secret (optional, use empty to remove)
* `OAUTH_REALM` - oAuth2 realm (optional, use empty to remove)
* `OAUTH_APP_NAME` - oAuth2 app name (optional, use empty to remove)
* `OAUTH_ADDITIONAL_PARAMS` - oAuth2 query string additional params (optional)

Variable value `**None**` should not be used for above defined variables.

## Run using local JSON file

If you want to use local JSON file to run swagger-ui you can do that by placing your `.json` file into container path `/app/swagger.json`

This can be executed in 2 ways:

* edit `Dockerfile` to `ADD` file into container
* map volume to `/app/swagger.json` from host machine

### Sample run with adding file to container

Contents of `Dockerfile`

```
FROM lukastosic/swaggerui-docker
ADD ./test-swagger.json /app/swagger.json
```

This `Dockerfile` assumes that `test-swagger.json` is in the same folder, if path is different you should adjust accordingly

Contents of `docker-compose.yml`

```
version: '2'
services:
  swagger_ui:
    build: .
    restart: always
    ports:
      - 80
```

This `docker-compose.yml` assumes that `Dockerfile` is in the same folder, if `Dockerfile` is in different location, then you have to adjust `build` path parameter


### Sample run with mapped volume

In this instance you don't need `Dockerfile`, only `docker-compose.yml`

```
version: '2'
services:
  swagger_ui:
    image: lukastosic/swaggerui-docker
    restart: always
    volumes:
      - ./test-swagger.json /app/swagger.json
    ports:
      - 80
```

This `docker-compose.yml` assumes that `test-swagger.json` is in the same folder, if path is different you should adjust accordingly

