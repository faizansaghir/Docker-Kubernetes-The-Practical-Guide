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
