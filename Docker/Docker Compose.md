---
date created: 2025-10-05 14:25
---

The docker compose is the toll that makes it easier to define and manage multi-container Docker applications. It allow us to define the services, networks, and volumes needed for our application in a simple YAMl file called `docker-compose.yml`.

## Components of a `docker-compose.yml` File

### **1. version**: Specifies the Compose File Version

```yaml
version: '3.8'
```

- **Purpose**: The `version` key defines the version of the Docker Compose file format you are using.

- **Details**: Docker Compose has multiple versions (such as `2`, `2.1`, `3`, `3.1`, etc.). Version `3.8` is the most recent and supports features like advanced networking, secrets, and more.

- **Why it’s Important**: Docker Compose evolves with each new version, adding new features and deprecating older ones. So, specifying the correct version ensures compatibility.

---

### **2. services**: Defines the Containers or Services

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
```

- **Purpose**: The `services` section defines the containers that will be run by Docker Compose. Each service corresponds to a container in the application.

- **Details**: Each service can have specific configurations like the image to use, ports to expose, volumes to mount, environment variables, and more.

#### Components inside a service:

1. **image**:
   - Specifies the Docker image to use for the service.
   - Example: `image: nginx:latest` uses the latest Nginx image from Docker Hub.
2. **build**:

   - This can be used as an alternative to `image` to build a Docker image from a `Dockerfile`.
   - Example:
```yaml
	build:
		context: .
		dockerfile: Dockerfile
```
3. **container_name**:

   - Defines the name of the container. By default, Docker Compose generates a name for the container using the project name and service name.

   - Example:

     ```yaml
     container_name: my-web-container
     ```
4. **ports**:

   - Maps ports from the host machine to the container.
   - Format: `"hostPort:containerPort"`.
   - Example: `8080:80` exposes port 80 of the container on port 8080 of the host.
5. **volumes**:

   - Mounts host directories or volumes into the container.
   - Format: `"hostPath:containerPath"`.
   - Example: `./html:/usr/share/nginx/html` mounts the `./html` directory from the host machine into the container at `/usr/share/nginx/html`.
   - Volumes are often used to persist data or to share data between the host and the container.
6. **environment**:

   - Sets environment variables inside the container.
   - Example:

 ```yaml
     environment:
       - MY_ENV_VAR=value
```
7. **depends_on**:

   - Specifies dependencies between services. Docker Compose will start services in the correct order.
   - Example:

```yaml
     depends_on:
       - db
```
8. **networks**:

   - Defines which Docker network(s) the service will connect to.

   - By default, all services are placed on the same network, but you can specify custom networks.
8. **restart**:
   - Specifies the restart policy for containers.
   - Values: `no`, `always`, `on-failure`, `unless-stopped`
   - Example:

```yaml
restart: always
```
10. **command**:
    - Overrides the default command for the container.
    - Example:


```
command:["nginx", "-g", "daemon off;"]
```

---

### **3. networks**: Defines Custom Networks

```yaml
networks:
  app-network:
    driver: bridge
```

- **Purpose**: The `networks` section defines the Docker networks used by the services. By default, Docker Compose creates a default network, but you can define custom networks if needed.
- **Details**: Networks enable communication between containers. Docker Compose connects all services to a default network unless otherwise specified.

#### Key elements of the `networks` section:

1. **driver**:
   - Specifies the network driver. Common values include `bridge`, `host`, and `overlay`.
     - `bridge`: The default network driver, which isolates containers from the host and each other.
     - `host`: Removes isolation between containers and the host machine, allowing containers to use the host network directly.
     - `overlay`: Used for multi-host networks in Swarm mode.
2. **name**:

   - Defines a custom name for the network.
   - Example: `app-network` in the example above.
1. **driver_opts**:
   - Allows customization of the network driver with additional options (e.g., subnet configuration).

---
### **4. volumes**: Defines Named Volumes

```yaml
volumes:
  db-data:
```

- **Purpose**: The `volumes` section defines persistent storage used by containers. Volumes are essential when you want to persist data between container restarts or share data across containers.
- **Details**: Volumes are especially useful for databases (e.g., MySQL, PostgreSQL) where data must survive container restarts.

#### Key elements of the `volumes` section:

1. **name**:
   - Defines the name of the volume (e.g., `db-data` in the example above). Docker will manage this volume.
1. **external**:
   - If set to `true`, Docker Compose will use an external volume that was created previously or externally to Docker Compose (e.g., volumes created manually or by other Compose files).
   - Example:

```yaml
   volumes:
     db-data:
      external: true
```
3. **driver**:

   - Specifies the volume driver to use. The default is `local`, but custom drivers can be used for advanced scenarios.
3. **driver_opts**:
   - Allows specifying options for the volume driver.

---

### **5. build**: Builds a Docker Image from a Dockerfile

```yaml
build:
  context: .
  dockerfile: Dockerfile
```

- **Purpose**: The `build` section is used to define how to build the Docker image for a service instead of using a pre-built image.
- **Details**: The `context` specifies the directory where Docker should look for the `Dockerfile`, and the `dockerfile` key is optional if the file is named `Dockerfile` and located in the context directory.

- **Key elements**:

  1. **context**: The build context, typically a directory that includes the Dockerfile and other necessary files.
  2. **dockerfile**: (Optional) The name of the Dockerfile (if different from the default `Dockerfile`).
  3. **args**: Specifies build-time arguments.
  4. **cache_from**: Allows using cached layers from other images during the build process.

---
### **6. depends_on**: Service Dependencies

```yaml
depends_on:
  - db
```

- **Purpose**: Defines service dependencies. Docker Compose ensures that the dependent services are started before the service that requires them.

- **Details**: While `depends_on` controls the order of service startup, it does **not** ensure that the dependent services are ready before starting the dependent service. For instance, a web server may start before a database is fully initialized. You may need to implement health checks or other startup mechanisms.

---

### **7. environment**: Set Environment Variables

```yaml
environment:
  - MYSQL_ROOT_PASSWORD: example
```

- **Purpose**: The `environment` section allows you to define environment variables that will be passed into the container during its startup.
- **Details**: This is useful for configuring containers, such as setting database credentials, API keys, etc.

---

### **8. healthcheck**: Defines a Health Check for Containers

```yaml
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost/health"]
  interval: 30s
  retries: 3
  start_period: 30s
  timeout: 10s
```

- **Purpose**: The `healthcheck` section defines a check to determine if the container is healthy.
- **Details**: Docker will periodically execute the specified command (e.g., `curl`) to check if the container is functioning properly. If the check fails a specified number of times, the container will be considered unhealthy.
