### Larvel and PHP Dockerized project

- **Concepts**
  - If no entrypoint is mentioned in Dockerfile, the base image entrypoint is used
  - When we specify `delegated` at end of bind mount instead of `ro` which is read only, it means the changes to the folder should be written in batches and not required to be instantly reflected. This improves performance
