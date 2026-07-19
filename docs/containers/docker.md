# Docker and Docker Compose Cheat Sheet

> **Applies to:** Current Docker Engine and Docker Compose v2
> **Last reviewed:** 2026-07-14

A quick reference for inspecting and operating containers, images, networks, volumes, and Compose applications.

## Safety

> [!WARNING]
> `rm`, `prune`, and Compose commands using `--volumes` can permanently remove containers or data. Inspect the target first and verify backups before deleting named volumes.

## Environment and status

```bash
docker version
docker info
docker context show
docker system df
```

## Containers

```bash
# List containers
docker ps
docker ps --all

# Run a disposable interactive container
docker run --rm -it <image> <command>

# Start, stop, and restart
docker start <container>
docker stop <container>
docker restart <container>

# Inspect state and resource use
docker inspect <container>
docker stats <container>
docker top <container>

# Logs
docker logs --tail 100 <container>
docker logs --follow --since 10m <container>

# Execute a command in a running container
docker exec -it <container> <command>

# Copy files
docker cp <container>:<container-path> <local-path>
docker cp <local-path> <container>:<container-path>
```

Remove a stopped container only after checking it:

```bash
docker inspect <container>
docker rm <container>
```

## Images

```bash
docker image ls
docker pull <image>:<tag>
docker image inspect <image>:<tag>
docker build --tag <repository>:<tag> .
docker tag <source-image> <registry>/<repository>:<tag>
docker push <registry>/<repository>:<tag>
docker image history <image>:<tag>
```

Remove an image:

```bash
docker image rm <image>:<tag>
```

## Networks

```bash
docker network ls
docker network inspect <network>
docker network create <network>
docker network connect <network> <container>
docker network disconnect <network> <container>
docker port <container>
```

## Volumes

```bash
docker volume ls
docker volume inspect <volume>
docker volume create <volume>
```

Before removing a volume, confirm that no container needs its data:

```bash
docker ps --all --filter volume=<volume>
docker volume rm <volume>
```

## Docker Compose v2

Compose v2 uses the integrated `docker compose` command. The older `docker-compose` executable is legacy syntax.

```bash
# Validate and render the effective configuration
docker compose config

# Start services
docker compose up --detach

# Show service state
docker compose ps

# Follow logs
docker compose logs --follow --tail 100

# Execute a command in a service container
docker compose exec <service> <command>

# Pull and rebuild
docker compose pull
docker compose build --pull

# Restart one service
docker compose restart <service>

# Stop services without removing containers
docker compose stop

# Remove Compose containers and default networks
docker compose down
```

`docker compose down` does **not** remove named volumes or images by default. Destructive variants must be explicit:

```bash
# Also remove named and anonymous volumes
docker compose down --volumes

# Also remove images created or used by the project
docker compose down --rmi local
```

## Cleanup

Preview disk usage first:

```bash
docker system df --verbose
docker container prune --filter until=24h
docker image prune
docker volume prune
```

> [!CAUTION]
> `docker system prune --all --volumes` can remove unused images and volumes containing data. Do not use it as routine housekeeping without reviewing the impact.

## Troubleshooting workflow

```bash
docker ps --all
docker inspect <container>
docker logs --tail 200 <container>
docker stats --no-stream <container>
docker network inspect <network>
docker events --since 10m
```

Check the container exit code and health status:

```bash
docker inspect --format '{{.State.Status}} exit={{.State.ExitCode}} health={{if .State.Health}}{{.State.Health.Status}}{{end}}' <container>
```

## Official references

- [Docker command-line reference](https://docs.docker.com/reference/cli/docker/)
- [Docker Compose command-line reference](https://docs.docker.com/reference/cli/docker/compose/)
- [docker compose down](https://docs.docker.com/reference/cli/docker/compose/down/)