---
date created: 2025-10-04 18:33
---

Docker is an open-source application that used to developer, ship and run application inside a light weight, portable, and isolated environment called containers

## Containers

The containers are the small, lightweight components that consists of all the necessary data (tools, source code, dependent files) that need to run the application. The container are useful as they are isolated from each other.

## From Bare Metal to Containers

### Bare Metal

A physical computer without any virtualisation. In the bare metal we install the operating system directly on the physical systems and run the application.

#### Key Features:

- No layers between hardware and OS.
- Best performance (nothing extra running).
- Full control over the machine.

#### Limitations:

- Only one OS can run at a time.
- Hard to scale or manage many apps on the same machine.
- Less flexible.
- **Hellish Dependencies**\
  Multiple applications share the same OS and system libraries. Different apps might need different versions of software or libraries, leading to conflicts and crashes.\
  This “dependency hell” makes managing software complex and fragile.

### Virtual Machines

The virtual machines is a virtual computer running inside a real computer. It uses a software layer called a hypervisor to split one physical computer into multiple virtual ones.
Each VM can have:

- Its **own OS**
- Its **own apps**
- Isolated from other VMs

#### How VMs Solved Bare Metal Issues:

| Bare Metal Problem        | VM Solution                                               |
| ------------------------- | --------------------------------------------------------- |
| Only one OS at a time     | Run multiple OSs on one machine (e.g., Linux + Windows)   |
| Poor resource usage       | Use one machine for many VMs, each with their own purpose |
| One app crash affects all | Each VM is isolated — one crash doesn’t affect others     |
| No app isolation          | Apps run in their own VMs, avoiding conflicts             |
| Expensive scaling         | Create new VMs without buying new hardware                |
| #### Problems with VMs:   |                                                           |

1. **Heavyweight** – Each VM runs a full OS, which takes up lots of disk space and memory.
2. **Slow Startup** – Booting a VM takes time (just like starting a full computer).
3. **Complex Management** – Managing lots of VMs can get tricky.

### Containers

A **container** is a **lightweight, portable environment** that includes:

- The application
- All its dependencies
- Configurations

Instead running a full os, containers share the host system's **OS Kernel**, making them much faster and smaller than VMs

#### Containers Solved VM Issues:

| VM Problem                    | Container Solution                                  |
| ----------------------------- | --------------------------------------------------- |
| Heavy (each VM has a full OS) | Containers are lightweight — no full OS             |
| Slow startup                  | Containers start in seconds                         |
| Complex scaling               | Containers are easy to duplicate and scale          |
| Hard to share apps            | Containers are portable and run the same everywhere |
| Dependency hell               | Each container has its own isolated environment     |
