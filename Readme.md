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

- **Creating Docker image**
  - Create a Dockerfile inside a directory with name `Dockerfile`. [Sample File](https://github.com/faizansaghir/Docker-Kubernetes-The-Practical-Guide/blob/main/Module%201/first-demo-starting-setup/Dockerfile)
  - Run `docker build .` while inside the directory where `Dockerfile` is present. This will build an image as per definition in `Dockerfile`
- **Starting a Docker Container**
  - Run command `docker run <image_id>` to run the image built
  - To map a port exposed by container, use `-p <host_port>:<container_port>` in `docker run` command. Example: `docker run -p 3000:3000 <image_id>`
- **Listing and stopping a Docker Container**
  - Run command `docker ps` to list all running containers
  - Run command `docker stop <container_name>` to stop the required container. The container name is present in output of command where we list running containers
- **Images vs Containers**
  - <img width="720" height="404" alt="images_vs_containers" src="https://github.com/user-attachments/assets/76b866b9-bb2e-48c0-b7ec-5bfae691562c" />
- **Running an image on container**
  - We can use one of the images in Docker hub or create our own image in our system and use it to spin a container
  - To run a container with some iamge, use: `docker run <image_id/image_name>` eg: `docker run node`
  - Some images expose a shell like `node` which when created using `docker run <image_id/image_name>` will exit without doing much
  - To list all containers(running and stopped), use: `docker ps -a` where `ps` -> process
  - To interact with shell of an image, use `docker run -it <image_id/ image_name>` where `-i` -> interactive and `-t` -> pseudo terminal
