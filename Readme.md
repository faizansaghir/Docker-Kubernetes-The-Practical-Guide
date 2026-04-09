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
- **Running an image on container**
  - To run a container with some iamge, use: `docker run <image_id/image_name>` eg: ` docker run 9f15553504fa81eec9f15553504fa81eecfc418a0dfff62f10e43a2a74c60fa3ab983bdcc3a2c0c54`
  - To interact with shell of an image, use `docker run -it <image_id/ image_name>` where `-i` -> interactive and `-t` -> pseudo terminal
- **Listing running docker containers**
  - To list all containers(running and stopped), use: `docker ps -a` where `ps` -> process and `-a` -> all containers(stopped also)
- **Stopping a container**
  - To stop a container, use `docker stop <container_name>` eg: `docker stop eager_chatterjee`
- **Publishing a port to local machine**
  - To publish a container port to one of local port, use: `docker run -p <local_port>:<container_port> <image_id/ image_name>`  where `-p` -> publish. eg: `docker run -p 3000:80 9f15553504fa81eecfc418a0dfff62f10e43a2a74c60fa3ab983bdcc3a2c0c54`
  - ***Note: An image is read-only i.e. once we build an image and we copy some files from our local to image, after building image, even if we change the file in our local, the image will still contain the file in its previous state before changes were made i.e. version of file when image was built. To update something, we need to rebuild the image after we change the file and the image created during build will be a new image all together***
  - Every instruction of Dockerfile represents a layer of docker image. While building an image, if docker has previously built an image without change upto some step of Dockerfile and nothing has changed up to that command, it will reuse the result from cache and only start re-evaluating from command where something has changed and subsequent commands that follow the command. Even file changes that are copied are detected by Docker during build
  - When we run a container, it creates a read/write layer on top of the image layer and the `CMD` command we provide executes
  - To optimize our build time, we can plan our Dockerfile definition smartly to use cached results for layers most of the time eg: When we have `COPY . /app` before `RUN npm install`, everytime we make a change to our source code, the cache will be invalidated for `RUN npm install` command also. Instead, if we have `COPY package.json /app` and then have `RUN npm install` followed by `COPY . /app`, we will use cached results till `RUN npm install` when our source code only changes, however, if we change `package.json`, it will invalidate the cache result from `COPY package.json /app` and subsequent layer. [An improved version of Dockerfile](https://github.com/faizansaghir/Docker-Kubernetes-The-Practical-Guide/blob/main/Module%202/nodejs-app-starting-setup-improved/Dockerfile)
