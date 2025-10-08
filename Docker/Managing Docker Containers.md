---
date created: 2025-10-05 14:15
---

### 1. **Exposing Ports & Container-to-Container Communication**

#### Exposing Ports

In Docker, exposing ports refers to making an internal container port accessible from the outside (i.e., from the host machine or other containers). This is done by mapping a container’s internal port to a port on the host.

**Syntax:**

```bash
docker run -p <host_port>:<container_port> <image_name>
```

For example:

```bash
docker run -p 8080:80 nginx
```

This will map port 80 inside the container (the default port for Nginx) to port 8080 on the host machine. Now, you can access Nginx on `http://localhost:8080`.

#### Container-to-Container Communication

In Docker, containers can communicate with each other, either on the same network or using Docker's default bridge network.

- **Bridge Network (Default)**: This is the default network for containers. Containers on the same bridge network can communicate with each other using their IP address or container name.

  To run multiple containers on the same bridge network:

  ```bash
  docker network create my-network
  docker run --network my-network --name container1 myimage
  docker run --network my-network --name container2 myimage
  ```

  Now, `container1` and `container2` can communicate with each other.

- **Docker Compose**: A more user-friendly way to manage multi-container applications, where containers can talk to each other by using the service name defined in `docker-compose.yml`.

  Example `docker-compose.yml`:

  ```yaml
  version: '3'
  services:
    app:
      image: myapp
      ports:
        - "8080:8080"
    db:
      image: mysql
      environment:
        MYSQL_ROOT_PASSWORD: rootpass
  ```

  In this case, the `app` container can communicate with the `db` container using the hostname `db`.

### 2. **Environment Variables & Secrets**

#### Environment Variables

Environment variables are often used to pass configuration values (e.g., API keys, database URLs) into a container. You can define them when running a container with the `-e` flag or through a Docker Compose file.

- **Via `docker run`**:

  ```bash
  docker run -e MY_ENV_VAR=value myimage
  ```

- **Via Docker Compose**:\
  In your `docker-compose.yml` file:

  ```yaml
  services:
    app:
      image: myapp
      environment:
        - MY_ENV_VAR=value
  ```

#### Secrets

Secrets are sensitive pieces of information (like passwords or API keys) that need to be securely stored and passed to a container. Docker provides a way to store secrets securely, particularly when using Docker Swarm, which has built-in support for managing secrets.

- **Creating a secret**:

  ```bash
  echo "mysecretpassword" | docker secret create my_secret -
  ```

- **Using a secret in Docker Compose** (only supported in Docker Swarm mode):

  ```yaml
  version: '3.7'
  services:
    app:
      image: myapp
      secrets:
        - my_secret
  secrets:
    my_secret:
      external: true
  ```

The secret will then be available in `/run/secrets/my_secret` inside the container. Only applications that need the secret can access it, and it’s stored in memory (not in a file system).