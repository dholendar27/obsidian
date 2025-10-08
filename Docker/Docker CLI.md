---
date created: 2025-10-04 18:31
---

Docker CLI (Command Line Interface) is the main way to interact with Docker, allowing you to manage containers, images, networks, and volumes. Here's a quick rundown of the basics:

### 1. **Docker Version**

Check the Docker version installed on your system.

```bash
docker --version
```

### 2. **Docker Help**

Get help on any Docker command.

```bash
docker --help
```

### 3. **Images**

- **List Images**\
  To list all Docker images on your machine:

  ```bash
  docker images
  ```

- **Pull Image**\
  Download an image from Docker Hub (or another registry):

  ```bash
  docker pull <image_name>
  ```

  Example:

  ```bash
  docker pull ubuntu
  ```

- **Build Image**\
  Build a Docker image from a `Dockerfile`:

  ```bash
  docker build -t <image_name> <path_to_dockerfile>
  ```

### 4. **Containers**

- **List Running Containers**\
  To see the containers that are currently running:

  ```bash
  docker ps
  ```

- **List All Containers**\
  To see all containers (including stopped ones):

  ```bash
  docker ps -a
  ```

- **Start a Container**\
  Start a container from an image:

  ```bash
  docker run -d --name <container_name> <image_name>
  ```

  Example:

  ```bash
  docker run -d --name my-ubuntu-container ubuntu
  ```

  `-d` means detached mode (running in the background).

- **Stop a Running Container**
  To stop a container:

  ```bash
  docker stop <container_name_or_id>
  ```

- **Remove a Container**
  To remove a container (even if it's stopped):

  ```bash
  docker rm <container_name_or_id>
  ```

- **Access a Container (Exec)**
  To open an interactive shell inside a running container:

  ```bash
  docker exec -it <container_name_or_id> bash
  ```

### 5. **Docker Logs**

View logs for a running or stopped container:

```bash
docker logs <container_name_or_id>
```

### 6. **Docker Networks**

- **List Networks**\
  Show the networks available:

  ```bash
  docker network ls
  ```

- **Create a Network**\
  Create a new custom bridge network:

  ```bash
  docker network create <network_name>
  ```

### 7. **Docker Volumes**

- **List Volumes**
  Show available volumes:

  ```bash
  docker volume ls
  ```

- **Create Volume**
  Create a new volume:

  ```bash
  docker volume create <volume_name>
  ```

- **Attach Volume**
  Attach a volume to a container at runtime:

  ```bash
  docker run -d -v <volume_name>:<path_in_container> <image_name>
  ```

### 8. **Prune (Cleanup)**

- **Remove Unused Images, Containers, Volumes, and Networks**\
  You can use the prune command to remove unused resources:

```bash
  docker system prune
```

  For a more selective cleanup:
```bash
  docker image prune
  docker container prune
  docker volume prune
```
