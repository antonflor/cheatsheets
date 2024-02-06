# Docker Command Cheat Sheet

This cheat sheet is intended for developers, DevOps engineers, and system administrators who use Docker for containerization. Docker is a popular platform for developing, shipping, and running applications in isolated environments called containers.

The commands listed here provide a comprehensive guide for managing Docker containers, images, volumes, networks, and more. This guide aims to streamline Docker operations and offer quick access to common commands necessary for effective container management and deployment.
Docker Commands

## Container Management

- **docker run [options] [image] [command]**
  - Runs a command in a new container.

- **docker ps**
  - Lists running containers.

- **docker ps -a**
  - Lists all containers, including stopped ones.

- **docker stop [container]**
  - Stops a running container.

- **docker start [container]**
  - Starts a stopped container.

- **docker restart [container]**
  - Restarts a container.

- **docker rm [container]**
  - Removes a container.

- **docker logs [container]**
  - Fetches logs of a container.

## Image Management

- **docker images**
  - Lists all downloaded images.

- **docker pull [image]**
  - Pulls an image from a registry.

- **docker push [image]**
  - Pushes an image to a registry.

- **docker rmi [image]**
  - Removes an image.

- **docker build -t [tag] .**
  - Builds an image from a Dockerfile in the current directory.

## Container Operations

- **docker exec -it [container] [command]**
  - Executes a command in a running container.

- **docker attach [container]**
  - Attaches to a running container.

- **docker cp [container]:[path] [local_path]**
  - Copies files from a container to the local filesystem.

- **docker rename [old_container_name] [new_container_name]**
  - Renames a container.

## Network Management

- **docker network ls**
  - Lists networks.

- **docker network create [options] [network]**
  - Creates a new network.

- **docker network rm [network]**
  - Removes a network.

- **docker network connect [network] [container]**
  - Connects a container to a network.

- **docker network disconnect [network] [container]**
  - Disconnects a container from a network.

## Volume and Storage

- **docker volume ls**
  - Lists all volumes.

- **docker volume create [volume]**
  - Creates a new volume.

- **docker volume rm [volume]**
  - Removes a volume.

- **docker volume inspect [volume]**
  - Inspects a volume.

## Docker Compose

- **docker-compose up**
  - Starts and runs the entire app defined in `docker-compose.yml`.

- **docker-compose down**
  - Stops and removes containers, networks, images, and volumes.

- **docker-compose build**
  - Builds or rebuilds services.

- **docker-compose logs [service]**
  - View output from containers.
