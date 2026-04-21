### Docker Compose

- **Overview**
  - Docker compose is tool to make deployment easier for containers and building image
  - It works with `Dockerfile` defined inside modules for building images and then helps replace complex multiple `docker run` and other docker commands
- **Using docker compose**
  - We can create a `docker-compose.yml` or `docker-compose.yaml` file
  - We then define the version of docker compose specification being used using `version: <specification-version>` eg: `version: "3.8"`. This allows docker to know which version is being used and what syntax will be followed and features that should be supported
  - We then define the services. These are definition for containers ou will create. See [docker-compose.yaml](https://github.com/faizansaghir/Docker-Kubernetes-The-Practical-Guide/blob/main/Module%206/docker-compose.yaml) for sample. Hierarchy
    ```
    services:
      <service-name>:
        <configuration for containers like image to be used, volumes, environment variables, environment variable file,   
          networks(only needed if we want to attach to some custom network else docker handles network part)>
    ```
  - Detached mode does not have equivalent command in docker compose, we can use `-d` while running docekr compose for detached mode
  - We do not need to specify remove flag in configuration as it is default bahvior of service when using docker compose. When we bring down docker compose, it removes the containers
  - Docker also creates a network and adds all the services to same network when we use docker compose to spin up containers
  - Named volumes are to be specified separately parallel to `services` under `volumes`. Hierarchy  
      ```
      volumes:  
      - <named-volume>:
      ```
  - To run docker compose use `docker-compose up` while in the folder iof `docker-compose.yaml`. This runs containers in attached mode and can be exited using `Ctrl+C` or by pressing `d` if terminal allows detaching using `d`
  - To run docker compose in detached mode, use `docker-compose up -d` and then bring down the setup using `docker-compose down`.
  - To delete named volumes also, use `docker-compose down -v`
- **Custom image container**
  - When we have custom image whose Dockerfile has been defined and is to be used in container, we use `build` instead of `image` in container configuration
  - If we need to set context and also specify name of the dockerfile if any custom name or have args in image, use hierarchy
    ```
    services
      <service-name>:
        build:
          context: <relative-path-to-docker-file> 
          dockerfile: <name-of-docker-file>
          args:
            <arg_1>: <val_1>
    ```
  - If we do not want any customization while specifying build, use hierarchy
    ```
    services
      <service-name>:
        build: <relative-path-to-docker-file>
    ```
  - We can specify the published ports using `ports`. Hierarchy
    ```
    services
      <service-name>:
        ports:
          - '<host-port>:<container-port>'
    ```
  - While specifying bind mounts in docker compose, we can specify relative paths
  - When we specify build instead of image, docekr does not always rebuild the image, it uses the image if present locally/ already build
  - We can also specify `depends_on` configuration where we specify list of services on which our current service depends on. Hierarchy
    ```
    services
      <service-name>:
        depends_on:
          - <other-service-name>
    ```
  - When we use docker compose to bring up containers/ services, it creates containers with `<folder-name_service-name>` but we can still use `<service-name>` to connect to the services like we still use `mongodb` to connect to mongo DB container
- **Interactive Mode for containers**
  - We can use `stdin_open` and `tty` flag inside container configuration to account for `-i` and `-t` flags respectively. Hierachy
    ```
    services
      <service-name>:
        stdin_open: true
        tty: true
    ```
  - After specifying the configuration, even when we run docker compose in detached mode, it will still make the required service start in interactive mode
- **Additional Docker Compose**
  - If we just want to build custom docker images and not bring up containers, we can use `docker-compose build`
  - If we want to force docekr to rebuild custom images even if locally present, use `docker-compose up --build`
  - We can also specify the container name which is to be created in container configuration. Hierarchy
    ```
    services
      <service-name>:
        container_name: <container-name>
    ```
  - Does the docker-compose command replace the docker command?  
      No, some commands like pushing image still goes thorugh `docker push` and we do not have docker compose alternate for this
