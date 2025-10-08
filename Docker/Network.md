---
date created: 2025-10-05 14:03
---

The docker network is the virtual network that containers use to communicate with each other. It's like a private network where containers can send and receive data. This makes it easier for different containers to interact with each other
There are different types of networks, and each one serves a different purpose.

## 1. Bridge Network

- **What it is**: This is the default network type. When you run a container without specifying a network, it connects to a bridge network.
- **How it works**: When you run a container in Docker, the container is placed in a virtual **network namespace**. A ==**bridge network**== is like a private "network switch" where containers can talk to each other, but it doesn't allow direct access to the outside world.

## 2. Host Network

- **What it is**: This network mode connects the container directly to the host machine’s network.
- **How it works**: Containers use the same IP address as the host. This means they can access all the services available on the host and the host can access the containers easily.
- **Use case**: It's useful when you need the container to have the same network performance as the host and don’t need any isolation.

```Dockerfile
docker run --network host -d nginx
```

## 3. Overlay Network

- **What it is**: This network type is used when you're running Docker in a **swarm** (which is Docker's way of managing a group of containers across multiple machines).
- **How it works**: Overlay networks allow containers on different physical machines to communicate with each other, as if they were on the same local network.
- **Use case**: Useful for large-scale applications running across multiple hosts, like when you want to manage a bunch of containers across different servers in a cluster.

> The docker the bridge network is used to communicate between the containers on the same host. but the overlay is used for communication between containers across multiple Docker host (like Docker swarm or kubernetes setup)

## 4. None Network

- **What it is**: When a container is connected to the "none" network, it has no network access at all.
- **How it works**: The container can't connect to any other containers or external networks.
- **Use case**: This might be used for containers that don’t need to communicate over a network.

## 5.  Macvlan Network

- **What it is**: A Macvlan network allows you to assign a unique MAC address to a container, making it appear as a physical device on the network.
- **How it works**: Containers are treated as if they are separate physical machines on the network, with their own IP addresses.
- **Use case**: This is used when you need a container to have direct access to the physical network and you need more control over the network interface.

### Key Concepts in Docker Networking

1. **IP Addressing**:
   - Each container gets an IP address when it's connected to a network. In a default bridge network, containers usually get an IP like `172.17.0.x`, and they can talk to each other using this address.
   - Containers on a **host network** don’t get their own IP address; they use the host's IP.
2. **Port Mapping**:
   - Containers typically run services on certain ports (like a web server running on port 80). To access these services from outside the container, you use **port mapping**.
   - Example: `docker run -p 8080:80` will map port 80 inside the container to port 8080 on the host machine. Now you can access the container’s web service through `http://host-ip:8080`.
3. **DNS Resolution**:
   - Docker provides an internal DNS system, which means you can refer to containers by their names. For example, if you have a container called `db`, other containers can access it by using `db` as the hostname, and Docker will resolve it to the correct IP address.
4. **Network Drivers**:
   - The network "driver" defines how the network will behave. The most common drivers are `bridge`, `host`, `overlay`, and `none`. You can also create custom drivers to meet specific needs.

Sure! Here's a breakdown of Docker commands to **create**, **list**, **inspect**, and **remove** networks, as well as manage their settings.

### 1. **Create a Network**

To create a network, you can specify the driver type (e.g., `bridge`, `overlay`, `host`, `macvlan`).

- **Create a Bridge Network (default)**:

  ```bash
  docker network create my_bridge_network
  ```

- **Create a Host Network**:

  ```bash
  docker network create --driver=host my_host_network
  ```

- **Create an Overlay Network** (for multi-host communication, typically used in Docker Swarm):

  ```bash
  docker network create --driver=overlay my_overlay_network
  ```

- **Create a Macvlan Network** (useful when you need containers to appear as physical devices):

  ```bash
  docker network create --driver=macvlan --subnet=192.168.1.0/24 my_macvlan_network
  ```

- **Create a None Network** (container will not have any network):

  ```bash
  docker network create --driver=none my_none_network
  ```

---

### 2. **List Networks**

To see all the networks available on your system, including default ones (`bridge`, `host`, `none`):

```bash
docker network ls
```

This will show a list of networks along with their ID, name, driver, and scope.

---

### 3. **Inspect a Network**

To view detailed information about a network (including the containers connected to it):

```bash
docker network inspect my_bridge_network
```

This will return JSON output that includes details like network settings, IP range, containers connected, etc.

---

### 4. **Connect a Container to a Network**

To connect an already running container to a specific network:

```bash
docker network connect my_bridge_network my_container_name
```

You can also specify the container's IP address if you need to assign one statically:

```bash
docker network connect --ip 192.168.1.100 my_bridge_network my_container_name
```

---

### 5. **Disconnect a Container from a Network**

To disconnect a container from a specific network:

```bash
docker network disconnect my_bridge_network my_container_name
```

---

### 6. **Remove a Network**

To remove an unused network:

```bash
docker network rm my_bridge_network
```

This will delete the network if no containers are using it. If containers are still connected, you’ll get an error, and you’ll need to disconnect them first.

---

### 7. **Prune Unused Networks**

If you want to remove all unused networks (those not connected to any containers), you can use:

```bash
docker network prune
```

This will prompt you for confirmation before deleting any unused networks. You can force it with `-f`:

```bash
docker network prune -f
```

---

### 8. **Manage Network Settings (Optional)**

You can adjust settings like subnet, gateway, and IP range when creating a custom network, especially for **bridge** or **macvlan** networks.

For example, to create a bridge network with a custom subnet and gateway:

```bash
docker network create \
  --driver=bridge \
  --subnet=192.168.100.0/24 \
  --gateway=192.168.100.1 \
  my_custom_bridge_network
```

---

### Example Workflow

- **Create a custom bridge network**:

  ```bash
  docker network create --driver=bridge --subnet=10.0.0.0/24 my_custom_network
  ```

- **Start a container and connect it to the network**:

  ```bash
  docker run -d --name=my_container --network my_custom_network nginx
  ```

- **Inspect the network**:

  ```bash
  docker network inspect my_custom_network
  ```

- **Disconnect the container**:

  ```bash
  docker network disconnect my_custom_network my_container
  ```

- **Remove the network**:

  ```bash
  docker network rm my_custom_network
  ```

---

Let me know if you'd like more specific examples or have questions about managing Docker networks!
