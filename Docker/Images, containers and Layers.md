---
date created: 2025-10-04 18:33
---

## Images

- A Docker image is a lightweight, standalone and executable packages that includes everything needed to run a piece of software-code, runtime, libraries, environment variables, and configuration files.
- Images are **read-only templates** used to create containers.
- They are built in layers; each instruction in a Dockerfile adds a new layer.
- You can think of an image as a snapshot or blueprint.

> **Docker Image** is like an **`.exe` file** that you can run on your computer. The `.exe` file contains all the instructions and resources needed to run a program, but it’s not doing anything on its own until you execute it.

---

## Containers

- A **Docker Container** is a runtime instance of a Docker image.
- It is the actual running process with the environment defined by the image.
- Containers are **isolated** from each other and the host system but share the OS kernel.
- Unlike images, containers are **read-write**, meaning any changes made while running are stored in the container.

> A **Docker Container** is like the **running process** of the `.exe` file. When you execute the `.exe` file, it starts doing work—just like a container runs a program. The container is **active**, **isolated**, and **uses resources** (like CPU, memory, and storage) while running.

---

## Layers

In Docker, **layers** are the building blocks of Docker images. Each layer represents a set of filesystem changes (like adding files, installing packages, modifying configurations) made by a single command in a Dockerfile.

- When you write a Dockerfile, each instruction (like `RUN`, `COPY`, `ADD`) creates a new layer.
- These layers stack on top of each other to form the final Docker image.
- Layers are **read-only**, except the top-most layer which is writable when the container runs.

### How It Works in Practice

- Suppose your Dockerfile starts with a base image layer (`FROM ubuntu`), then adds an app layer (`COPY app /app`), then installs dependencies (`RUN apt-get install ...`).
- These layers form a stack, and Docker uses union filesystem techniques to present them as a single image.
- When you run a container, Docker adds a **writable layer** on top where changes made during runtime are stored.
