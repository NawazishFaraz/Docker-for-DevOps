# Docker Commands Cheat Sheet

This document provides a comprehensive list of Docker commands with a brief description of their functionality.

## 1. Docker Installation and Version
- **`docker --version`**  
  Displays the installed Docker version.

- **`docker info`**  
  Shows detailed information about the Docker installation.

## 2. Docker Images
- **`docker images`**  
  Lists all images on the local machine.

- **`docker pull <image_name>`**  
  Downloads an image from Docker Hub or a specified registry.

- **`docker rmi <image_name>`**  
  Removes a specific image from the local machine.

- **`docker build -t <tag_name> .`**  
  Builds an image from a Dockerfile in the current directory and tags it.

## 3. Docker Containers
- **`docker ps`**  
  Lists all running containers.

- **`docker ps -a`**  
  Lists all containers, including stopped ones.

- **`docker run <image_name>`**  
  Creates and starts a container from a specified image.

- **`docker run -d <image_name>`**  
  Runs a container in detached mode (in the background).

- **`docker run -it <image_name>`**  
  Runs a container interactively with a TTY.

- **`docker stop <container_id>`**  
  Stops a running container.

- **`docker start <container_id>`**  
  Starts a stopped container.

- **`docker rm <container_id>`**  
  Removes a stopped container.

- **`docker logs <container_id>`**  
  Fetches logs of a container.

- **`docker exec -it <container_id> <command>`**  
  Executes a command inside a running container.

- **`docker inspect <container_id>`**  
  Provides detailed information about a container.

## 4. Docker Volumes
- **`docker volume create <volume_name>`**  
  Creates a new volume.

- **`docker volume ls`**  
  Lists all volumes.

- **`docker volume rm <volume_name>`**  
  Removes a specified volume.

- **`docker run -v <volume_name>:<container_path> <image_name>`**  
  Mounts a volume to a container.

## 5. Docker Networks
- **`docker network ls`**  
  Lists all Docker networks.

- **`docker network create <network_name>`**  
  Creates a new network.

- **`docker network connect <network_name> <container_id>`**  
  Connects a container to a specified network.

- **`docker network disconnect <network_name> <container_id>`**  
  Disconnects a container from a network.

## 6. Docker Compose
- **`docker-compose up`**  
  Starts the services defined in a `docker-compose.yml` file.

- **`docker-compose down`**  
  Stops and removes the services defined in the compose file.

- **`docker-compose logs`**  
  Fetches logs for all services.

- **`docker-compose ps`**  
  Lists the status of all services defined in the compose file.

## 7. Docker System
- **`docker system df`**  
  Displays disk usage by Docker.

- **`docker system prune`**  
  Removes unused images, containers, volumes, and networks.

- **`docker system info`**  
  Shows detailed system-wide information.

---

## Notes
- Replace `<image_name>`, `<container_id>`, `<volume_name>`, or `<network_name>` with the appropriate names/IDs for your use case.
- Use the `--help` option with any Docker command to see its detailed usage.
