### Larvel and PHP Dockerized project

- **Concepts**
  - If no entrypoint is mentioned in Dockerfile, the base image entrypoint is used
  - When we specify `delegated` at end of bind mount instead of `ro` which is read only, it means the changes to the folder should be written in batches and not required to be instantly reflected. This improves performance
  - Initializing a `Laravel` project using `composer` use `docker-compose run --rm composer create-project --prefer-dist laravel/laravel .`
  - To bring up only selected services from docker compose, use `docker-compose up <services>` eg: `docker-compose up -d server php mysql`
  - If we bring up a service using `docker-compose up <service>`, it also brings up all services that the mentioned service depends on also any sub services the dependency service depends on eg: `docker-compose up -d --build server` brings up `php` as it is a dependency which has dependency on `mysql` hence that brings up MySQL
  - We can also override entrypoint or working directory using `entrypoint` or `working_dir` in docker-compose file. Hierarchy
  ```
  services:
    <service-name>:
      entrypoint: ["part1", "part2"]
  ```
  - We do not have any such configuration for `RUN` and `COPY` etc. in docker compose file
