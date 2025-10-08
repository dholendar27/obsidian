---
date created: 2025-10-04 18:33
---

A **Dockerfile** is a script containing a set of instructions on how to build a Docker image. It is essentially a blueprint for creating Docker containers. Dockerfiles define the environment, dependencies, and commands necessary to run an application in a container.

## Dockerfile Commands Explained

### 1. FROM

The `FROM` instruction in a Dockerfile specifies the base image from which we are building your new Docker image. It is the fundamental starting point for any Docker image creation.

> Without a **base image**, you **cannot run** a Dockerfile.

```Dockerfile
FROM ubuntu:20.04
```

This command tells Docker to use the `ubuntu:20.04` image as the base image from the container. The `FROM` instruction must be the first instruction in a Dockerfile

### 2. LABEL

Labels are used to add metadata to the Docker image, such as the maintainer information, version, or description.

```Dockerfile
LABEL maintainer="John Doe <john.doe@example.com>"
LABEL version="1.0"
LABEL description="This is a sample application."
```

### 3. RUN

The `RUN` instruction executes commands during the build process, creating a new layer in the Docker image.

```Dockerfile
RUN apt-get update && apt-get install -y python3
```

This command runs the `apt-get` commands to install Python 3. Each `RUN` command creates a new layer in the Docker image, so minimizing them helps reduce image size.

### 4. COPY

The `COPY` instruction copies files from the host machine into the container image.

```Dockerfile
COPY ./app /app
```

This command copies the `app` folder from your host machine to the `/app` directory in the container. You can also use a `--chown` flag to set ownership of copied files, for example:

```Dockerfile
COPY --chown=user:group ./app /app
```

### 5. ADD

`ADD` is similar to `COPY` but more powerful. It can handle tarball archives and also supports remote URLs.

```Dockerfile
ADD https://example.com/archive.tar.gz /app
```

This command downloads and extracts the tar file to the `/app` directory in the container.
While `ADD` is more powerful, it’s recommended to use `COPY` unless you specifically need the additional functionality of `ADD`.

### 6. WORKDIR

The `WORKDIR` instruction sets the working directory inside the container. If the directory doesn’t exist, it is created.

```Dockerfile
WORKDIR /app
```

This command sets the working directory to `/app` for subsequent instructions. Any relative paths used in later commands will be relative to this directory.

### ENV

The `ENV` instruction sets environment variables in the container.

```Dockerfile
ENV APP_ENV=production
```

This command sets the environment variable `APP_ENV` to `production`, which can be accessed by your application running inside the container.

### 8. EXPOSE

The `EXPOSE` command informs Docker that the container will listen on the specified network ports at runtime.

```Dockerfile
EXPOSE 80
```

This command exposes port `80` in the container, which is useful for web applications. **Note**: This does not publish the port on the host system; it’s just a way to indicate which port the application will use.

#### What `EXPOSE` Does

The `EXPOSE` instruction in a Dockerfile **does not** actually publish a port or make it “used” by the container. Instead, it serves two purposes:

1. **Informational**: It tells Docker and anyone who uses the image that the container is expected to listen on the specified port(s). It's a kind of **documentation** within the Dockerfile about which ports the application inside the container will use.
   For example:

```Dockerfile
EXPOSE 80
```

This just indicates that the container will listen on port `80` (commonly used for web servers like Nginx or Apache). But this does **not** open or bind the port in any way.

2. **Networking and Communication**: In a Docker Compose setup or a Docker network, exposing ports can help other containers communicate with the container that has the exposed port. But on its own, it does not make the port accessible from outside the container unless you **explicitly publish** the port when running the container.

### 9. CMD

The `CMD` instruction specifies the command to run when the container starts. It can take the form of an array (exec form), a string (shell form), or a default command.

```Dockerfile
# Exec form
CMD ["python3", "app.py"]  # Preferred, runs directly without shell

# Shell form
CMD python3 app.py  # Runs in a shell, less efficient

# Default command with ENTRYPOINT (useful for flexible command arguments)
ENTRYPOINT ["python3"]
CMD ["app.py"]  # Default arguments for ENTRYPOINT
```

### ENTRYPOINT

`ENTRYPOINT` defines the executable that will run when the container starts. It is similar to `CMD`, but more restrictive. If both `ENTRYPOINT` and `CMD` are used, `CMD` will provide default arguments to the `ENTRYPOINT`.

```Dockerfile
ENTRYPOINT ["python3", "app.py"]
```

This ensures that `python3 app.py` is always run, and you can append additional arguments if needed.

## Instructions that creates a layer

- Instructions that create layers:
  - `FROM`, `COPY`, `ADD`, `RUN`, `WORKDIR`, `USER`, `ENV`, `VOLUME`, `EXPOSE`, `ENTRYPOINT`, `CMD`, `LABEL`
- Instructions that do **not** create layers:
  - `ARG`, `SHELL`
