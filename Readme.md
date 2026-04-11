# Docker And Kubernetes

## Docker

### Containers and Virtual Machines

- **Virtual Machines on Machine**
  - <img width="720" height="404" alt="vms_on_machine" src="https://github.com/user-attachments/assets/0ae71967-063a-4cff-b02f-6e13f572de36" />
- **Containers on Machine**
  - <img width="720" height="404" alt="containers_on_machine" src="https://github.com/user-attachments/assets/5648a60b-201e-4cab-bf18-e87408d83fb5" />
- **Containers vs Virtual Machines**
  - <img width="720" height="406" alt="vm_vs_container" src="https://github.com/user-attachments/assets/267847b4-9a39-4471-8c6e-a8093f56d657" />

### Basics of Docker

- **Images vs Containers**
  - <img width="720" height="404" alt="images_vs_containers" src="https://github.com/user-attachments/assets/76b866b9-bb2e-48c0-b7ec-5bfae691562c" />
- **Creating custom Docker image**
  - Dockerfile contains details of how we want to build our custom Docker image
  - `FROM` allows you to build our image on top of existing/ available image. We can build from scratch without `FROM` i.e. without building on top of other docker image, but we usually take some base OS or other image to start. The `FROM <image_id/ image_name>` can be of any image that exists on DockerHub or on our local system
  - `WORKDIR` to specify which is the workin directory to be set as context with syntax `WORKDIR <path_inside_image>` eg: `WORKDIR /app` to make `/app` as current working directory
  - `COPY` to copy a file from local to some path inside image with syntax `COPY <local_path> <image_path>` eg: `COPY . /app` means copy all files from current folder to `/app` path in the image. We can also use `COPY . ./` to copy all files from current directory where Dockerfile is to current working directory of image
  - `RUN` to run a command in the image while cuilding the image with syntax `RUN <command>` eg: `RUN npm install`. This will run command with current working directory of image as context
  - `EXPOSE` to expose a port from container to external world with syntax `EXPOSE <port>` eg: `EXPOSE 80`. This is a best practice followed but is not necessary to write this command as the ports are already exposed by default
  - `CMD` to run a command when a container is created using the image. The command will then run when container is created and image is deployed on it. Syntax `CMD [<command>]` eg: `CMD ["node","server.js"]`
  - See example of [Dockerfile](https://github.com/faizansaghir/Docker-Kubernetes-The-Practical-Guide/blob/main/Module%202/nodejs-app-starting-setup/Dockerfile)
  - To use the Dockerfile to build a docker image use `docker build <path_to_dockerfile>` eg: `docker build .` tells docker to use `Dockerfile` residing in current directory
  - At the end of execution of `docker build` command, you will get the ID of the image generated
- **Listing images available locally**
  - To list all images present on local(downloaded from hub or built locally), use `docker images`. This will only include tagged images
  - To list all images tagged or untagged, use `docker images -a` where `-a` -> all
- **Running an image on container**
  - To run a container with some iamge, use: `docker run <image_id/image_name>` eg: ` docker run 9f15553504fa81eec9f15553504fa81eecfc418a0dfff62f10e43a2a74c60fa3ab983bdcc3a2c0c54`
  - To interact with shell of an image, use `docker run -it <image_id/ image_name>` where `-i` -> interactive and `-t` -> pseudo terminal
- **Listing docker containers**
  - To list running docker containers use `docker ps` where `ps` -> process
  - To list all containers(running and stopped), use: `docker ps -a` where `-a` -> all containers(stopped also)
- **Stopping a container**
  - To stop a container, use `docker stop <container_name>` eg: `docker stop eager_chatterjee`
- **Publishing a port to local machine**
  - To publish a container port to one of local port, use: `docker run -p <local_port>:<container_port> <image_id/ image_name>`  where `-p` -> publish. eg: `docker run -p 3000:80 9f15553504fa81eecfc418a0dfff62f10e43a2a74c60fa3ab983bdcc3a2c0c54`
- **Images and Image Layers**
  - An image is read-only i.e. once we build an image and we copy some files from our local to image, after building image, even if we change the file in our local, the image will still contain the file in its previous state before changes were made i.e. version of file when image was built. To update something, we need to rebuild the image after we change the file and the image created during build will be a new image all together
  - Every instruction of Dockerfile represents a layer of docker image. While building an image, if docker has previously built an image without change upto some step of Dockerfile and nothing has changed up to that command, it will reuse the result from cache and only start re-evaluating from command where something has changed and subsequent commands that follow the command. Even file changes that are copied are detected by Docker during build
  - When we run a container, it creates a read/write layer on top of the image layer and the `CMD` command we provide executes
  - To optimize our build time, we can plan our Dockerfile definition smartly to use cached results for layers most of the time eg: When we have `COPY . /app` before `RUN npm install`, everytime we make a change to our source code, the cache will be invalidated for `RUN npm install` command also. Instead, if we have `COPY package.json /app` and then have `RUN npm install` followed by `COPY . /app`, we will use cached results till `RUN npm install` when our source code only changes, however, if we change `package.json`, it will invalidate the cache result from `COPY package.json /app` and subsequent layer. [An improved version of Dockerfile](https://github.com/faizansaghir/Docker-Kubernetes-The-Practical-Guide/blob/main/Module%202/nodejs-app-starting-setup-improved/Dockerfile)
- **Starting a stopped container**
  - We can also start a stopped container using `docker start <container_name>` eg: `docker start heuristic_greider`, this restarts the container.
- **Atached and Detached mode**
  - For `docker run` attached mode is default while for `docker start` detached mode is default.
  - Attached mode means we are listening to output of container i.e. things being printed on console, so any console.log or similar statement will be shown when executed
  - To run container in detached mode for new container, use `docker run -d <image_id>` where `-d` -> detached eg: `docker run 
-d -p 8080:80 3ea9121a2c90167dd755f61984f3fb9d2fd4c4cb116af018165a08383a548199`
  - To attach back to a detached container to see logs from current time and future logs, use `docker attach <container_name>` eg: `docker attach d7b7a1a4b536`
  - To restart a container in attached mode, use `docker start -a <container_name>` where `-a` -> attached eg: `docker start -a heuristic_greider`
- **Getting logs of container**
  - To get past logs, use `docker logs <container_name>` eg: `docker logs heuristic_greider`
  - To get past logs and also follow future logs, use `docker logs -f <docker_name>` eg: `docker logs -f heuristic_greider`
- **Interactive mode**
  - Docker can be used to docarize simple utility applications like calculator also apart from long running process like web servers eg: [Random number generator](https://github.com/faizansaghir/Docker-Kubernetes-The-Practical-Guide/blob/main/Module%202/python-app-starting-setup/rng.py)
  - When a program running inside container needs input from user, it needs to be in interactive mode so that STDIN can be open for the required input
  - To create a new container in interactive mode, use `docker run -i <docker_image>` eg: `docker run -i 53ac6e148dbf63719fe047fe8312bf30c71fe3774a91dcb4a2287e7229050686`
  - We should not have interactive mode with detach option as it will simply detach the container and you will not be able to input anything
  - To restart a container in interactive mode, use `docker start <container_name>` eg: `docker start -i 690583c16bf2`
- **Cleaning up stopped containers**
  - To remove a stopped container from memory and storage completely, use `docker rm <container_name>` eg: `docker rm thirsty_curran`
  - We can also remove multiple containers using space separated container names eg: ` docker rm zealous_banach tender_golick mystifying_murdock`
  - We can only remove containers which are not running, if we try to remove a running container, we get error
- **Cleaning up unused images**
  - To remove an usused image, use `docker rmi <image_id>` eg: `docker rmi 055ca80cc65c`
  - We can also remove multiple images using space separated image ids eg: `docker rmi 3ea9121a2c90 253dd19a27ed 52144f5e65a7`
  - We can only remove images that are not being used either by running or stopped containers i.e. the image should not be in use or referenced by a container
  - To remove all unused images without tag, use `docker image prune`. To delete all images with or without tag, use `docker image prune -a`
- **Automatically removing containers when stopped**
  - To specify that a container should be removed on stopping, use `docker run --rm <docker_image>` where `--rm` -> remove eg: `docker run -p 3000:80 -d --rm 9f15553504fa`
- **Getting details of an image**
  - To get details of image like date created, layers, entrypoint, env variables, port exposed etc, use: `docker inspect <docker_image>` eg: `docker inspect 9f15553504fa`
- **Copy file into and out of a container**
  - We can copy file from local to a running container using `docker cp <source_path> <container_name>:<destination_path>` eg: `docker cp ..\..\test.txt serene_golick:/app/`
  - We can copy file from cintainer to local using `docker cp <container_name>:<source_path> <destination_path>` eg: `docker cp serene_golick:/app/test.txt .`
  - The folder wherer we are copying files should be present in container for command to execute successfully
  - This can be used to update a file that is not currently running inside container to update for any changes and can also be used to get files like log files outside the container
- **Naming and tagging images and containers**
  - To give a custom name to container instead of using randomly generated name, we can use: `docker run --name <container_name> <docker_image>` eg: `docker run -d --rm -p 3000:80 --name webapp 9f15553504fa`. We can then use this name for stoping and removing container if needed
  - An image has 2 parts to its name `<repository>:<tag>` where repository is something that tells the group of image like `node` while tag specified a specific version of image. Use `docker build -t <repository>:<tag> <dockerifle_path>` to build an image with a specific tag eg: `docker build -t webapp:latest .`
  - We can use the tag we provided to run a container with the image we created eg: `docker run -d --rm -p 3000:80 --name webapp-container webapp:latest`
  - To rename an image use `docker tag <old_image_tag> <new_image_tag>` where we will create a new image as a copy of old image and both will exist eg: `docker 
tag bmi-calculator:latest faizansaghir/bmi-calculator`
- **Sharing docker images**
  - We can share the docker image either by providing the Dockerfile and the required files to build the docker image or we can directly share the docker image using any image hosting service like Docker Hub(official Docker image repository)
  - We can specify if an image can be public or private. Official images like `python` and `node` need authentication and other process for upload while custom images do not
  - To push an image to Docker Hub use `docker push <image_name>` or in case of repository other than Docker Hub `docker push <repository_name>:<image_name>`
  - To push an image to Docker Hub, we need to create an account in DockerHub, create a repository for that image repository or group and while creating image, we need to follow convention for tag as `<username>/<repository_name>:<tag>` eg: `docker push faizansaghir/bmi-calculator:latest`
- **Authenticating Local for DockerHub**
  - To login and authenticate your local system to Docker repository, use `docker login`
  - To logout your user from local, use `docker logout`
