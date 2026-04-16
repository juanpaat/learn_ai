# Docker: A Beginner's Guide

> A practical, plain-English introduction to Docker for developers and data scientists.

---

## Table of Contents

1. [Containerization and Virtualization](#1-containerization-and-virtualization)
2. [What is Virtualization?](#2-what-is-virtualization)
3. [What is a Virtual Machine (VM)?](#3-what-is-a-virtual-machine-vm)
4. [What is Docker and Why Does It Exist?](#4-what-is-docker-and-why-does-it-exist)
5. [Docker vs Virtual Machines](#5-docker-vs-virtual-machines)
6. [Installing Docker](#6-installing-docker)
7. [Images](#7-images)
8. [Containers](#8-containers)
9. [Running Containers](#9-running-containers)
10. [Essential Docker Commands](#10-essential-docker-commands)
11. [Creating Your Own Image (Dockerfile)](#11-creating-your-own-image-dockerfile)
12. [Example: Packaging a Python Regression Pipeline](#12-example-packaging-a-python-regression-pipeline)
13. [Image Versioning and Tagging](#13-image-versioning-and-tagging)
14. [The Docker Workflow in Data Science](#14-the-docker-workflow-in-data-science)

---

## 1. Containerization and Virtualization

Imagine you build a machine learning model on your laptop. It works perfectly. You send it to a colleague and they run it — it crashes. The error message says a package is missing or the Python version is different.

This is one of the oldest frustrations in software: **"it works on my machine."**

The core problem is that software depends on its **environment** — the operating system, installed libraries, Python version, system tools, and configuration. When the environment differs, behavior differs.

Two major technologies were invented to solve this: **virtualization** and **containerization**.

- **Virtualization** creates a full copy of an operating system inside another — completely isolated, but heavy.
- **Containerization** isolates just the application and its **dependencies** (the external libraries, tools, and runtimes the application needs to run) — lightweight, fast, and portable.

Docker is the dominant containerization technology today. Before understanding Docker, it helps to understand what came before it.

---

## 2. What is Virtualization?

**Virtualization** is the process of creating a software-based (virtual) version of something that normally exists as hardware — like a computer, a storage device, or a network.

In the context of computing, **server virtualization** lets you run multiple independent operating systems on a single physical machine at the same time. Each of these virtual environments thinks it's a real computer.

The software that makes this possible is called a **hypervisor**. The hypervisor sits between the physical hardware and the virtual machines, managing how each one gets access to CPU, memory, and disk.

There are two types of hypervisors:

| Type | Description | Examples |
|------|-------------|---------|
| Type 1 (bare-metal) | Runs directly on hardware, no host OS needed | VMware ESXi, Microsoft Hyper-V, Xen |
| Type 2 (hosted) | Runs on top of an existing operating system | VirtualBox, VMware Workstation |

```
Physical Hardware
      │
  Hypervisor
  ┌───┴────┐
  VM 1    VM 2
(Linux) (Windows)
```

Virtualization was a revolution for data centers — instead of buying 10 physical servers, you could run 10 virtual machines on one. But each virtual machine carries significant overhead, as you'll see next.

---

## 3. What is a Virtual Machine (VM)?

A **Virtual Machine (VM)** is a complete, self-contained computer running inside your actual computer. It has its own virtualized CPU, RAM, disk, and network card — and most importantly, it runs its own full **operating system**.

### Real-world examples

- **VirtualBox** (free, open source): Run Ubuntu Linux on your Windows or Mac laptop.
- **VMware Workstation / Fusion**: Run multiple OSes for testing or development.
- **AWS EC2 / Azure VMs / Google Compute Engine**: Cloud providers rent you virtual machines by the hour. When you spin up an EC2 instance, you're getting a VM on Amazon's physical servers.
- **macOS on Apple Silicon**: Apple's Virtualization framework lets you run Linux VMs natively.

### What's inside a VM?

```
Your Physical Machine (Host)
├── Host Operating System (e.g., macOS)
│   └── Hypervisor (e.g., VirtualBox)
│       ├── VM 1
│       │   ├── Guest OS (Ubuntu 22.04) ← full Linux kernel
│       │   ├── Libraries and dependencies
│       │   └── Your application
│       └── VM 2
│           ├── Guest OS (Windows 11)
│           └── Another application
```

### The downside

Each VM includes a **full operating system** — typically 1–20 GB in size, requires minutes to boot, and consumes significant CPU and RAM even when idle. If you need to run 50 isolated services, running 50 VMs becomes extremely expensive in terms of resources.

This is the gap Docker was built to fill.

---

## 4. What is Docker and Why Does It Exist?

**Docker** is an open-source platform for building, shipping, and running applications in **containers**.

### The problem Docker solves

Software has dependencies — your Python script needs Python 3.11, pandas 2.0, scikit-learn 1.4, and a dozen other packages. If someone else runs it with a different setup, things break. Historically, developers managed this with:

- `requirements.txt` — helps with Python packages, but not Python itself or system libraries.
- Virtual environments (`venv`, `conda`) — helps per project, but only on the same machine.
- VMs — solves the problem completely, but is too heavy for most use cases.

Docker finds the sweet spot: it **packages your application and everything it needs** (runtime, libraries, environment variables, config files) into a single portable unit — a **container** — that runs identically on any machine that has Docker installed.

### A useful analogy

Think of Docker containers like **shipping containers** in cargo transport.

Before standardized shipping containers, every ship, truck, and crane had to handle goods differently — bananas loaded differently than cars or grain. The invention of the standardized steel container transformed logistics: a container loaded in Shanghai can be picked up by a truck in Hamburg without anyone touching the contents.

Docker containers do the same for software: you pack your application once, and it runs the same way everywhere — your laptop, a colleague's machine, a test server, or a cloud cluster.

### Why Docker became dominant

Docker was released in 2013 and quickly became the industry standard for containers because it:

- Made containers easy to build and share (via `Dockerfile` and Docker Hub).
- Introduced a simple CLI and an intuitive workflow.
- Worked on Linux, macOS, and Windows.
- Integrated naturally with CI/CD pipelines and cloud platforms (AWS, GCP, Azure).

---

## 5. Docker vs Virtual Machines

Now that you know both, here's how they compare:

| Feature | Virtual Machine | Docker Container |
|--------|----------------|-----------------|
| Includes full OS | Yes (GBs) | No (shares host kernel) |
| Startup time | Minutes | Seconds (or milliseconds) |
| Size | GBs | MBs |
| Isolation level | Very strong (hardware-level) | Strong (process-level) |
| Performance overhead | High | Low |
| Portability | Moderate | Very high |
| Resource usage | High | Minimal |
| Use case | Full OS isolation, legacy apps, different kernels | App packaging, microservices, CI/CD, data science pipelines |

### How containers achieve isolation without a full OS

VMs virtualize the **hardware** — every VM has its own OS kernel.

Containers virtualize the **operating system** — every container shares the host machine's OS kernel, but each one has its own isolated filesystem, processes, and network. This is done using two Linux kernel features:

- **Namespaces**: isolate what a container can see (processes, network, filesystem).
- **cgroups**: limit how much CPU and memory a container can use.

```
Virtual Machines                    Docker Containers
─────────────────                   ─────────────────
 Physical Hardware                   Physical Hardware
       │                                   │
   Hypervisor                         Host OS (Linux)
  ┌────┴────┐                        ┌─────┴─────┐
  VM 1     VM 2                   Container 1  Container 2
  ├─OS─┐   ├─OS─┐                 ├─App libs─┐  ├─App libs─┐
  │App │   │App │                 │  App     │  │  App     │
  └────┘   └────┘                 └──────────┘  └──────────┘
(Each has its own kernel)        (Kernel is shared)
```

### When to use which

- Use a **VM** when you need full OS isolation, a different kernel (e.g., running Windows apps on Linux), or maximum security boundaries.
- Use **Docker** for application deployment, data science environments, microservices, testing, and CI/CD pipelines.

In practice, most modern cloud deployments run **containers inside VMs** — VMs provide infrastructure isolation, containers provide application isolation on top of that.

---

## 6. Installing Docker

### Docker Engine vs Docker Desktop

There are two ways to install Docker:

| | Docker Engine | Docker Desktop |
|---|---|---|
| Platform | Linux only | macOS, Windows, Linux |
| Interface | CLI only | CLI + graphical UI |
| Best for | Servers, CI/CD | Local development |
| License | Free (Apache 2.0) | Free for personal/small teams |

For local development on macOS or Windows, **Docker Desktop** is the recommended starting point. It bundles:
- Docker Engine
- Docker CLI
- Docker Compose
- A GUI to view containers and images
- A built-in Kubernetes cluster (optional)

### Install Docker Desktop (macOS / Windows)

1. Go to [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
2. Download the installer for your OS.
3. Run the installer and follow the prompts.
4. Launch Docker Desktop — you'll see the whale icon in your menu bar or taskbar.

### Install Docker Engine (Linux)

```bash
# Update package index
sudo apt-get update

# Install dependencies
sudo apt-get install ca-certificates curl gnupg

# Add Docker's official GPG key and repository
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

# Allow running Docker without sudo
sudo usermod -aG docker $USER
newgrp docker
```

### Verify the installation

```bash
docker --version
# Docker version 27.x.x, build ...

docker run hello-world
# Pulls a test image and runs it — you should see a "Hello from Docker!" message
```

If that works, Docker is installed and running correctly.

---

## 7. Images

Before you can run a container, you need an **image**. An image is the blueprint — a static, read-only snapshot of everything needed to run an application:

- A base operating system layer (e.g., Ubuntu, Alpine Linux, or just `debian:slim`)
- Language runtimes (e.g., Python 3.11)
- Libraries and dependencies (e.g., pandas, scikit-learn)
- Your application code
- Configuration (environment variables, default commands)

### Images are made of layers

Docker images are built in **layers**, where each layer represents a change on top of the previous one. This is efficient: layers are cached and reused across images.

```
Layer 4: COPY train.py /app/         ← your code
Layer 3: RUN uv pip install -r requirements.txt
Layer 2: COPY requirements.txt /app/
Layer 1: FROM python:3.11-slim        ← base image (also layers)
```

If you rebuild the image after only changing `train.py`, Docker reuses layers 1–3 from cache and only rebuilds layer 4. This makes builds fast.

### Where do images come from?

- **Docker Hub** ([hub.docker.com](https://hub.docker.com)): The default public registry. Thousands of official images exist here — `python`, `postgres`, `nginx`, `ubuntu`, etc.
- **Private registries**: Your company might host images on AWS ECR, Google Artifact Registry, or GitHub Container Registry.
- **Built locally**: You create your own image from a `Dockerfile` (covered in section 11).

### Pulling an image

```bash
# Pull the official Python 3.11 image
docker pull python:3.11-slim

# Pull a specific PostgreSQL version
docker pull postgres:16
```

### Listing local images

```bash
docker images
# REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
# python        3.11-slim a1b2c3d4e5f6   2 weeks ago    130MB
# postgres      16        f6e5d4c3b2a1   3 weeks ago    425MB
```

---

## 8. Containers

A **container** is a running instance of an image.

The relationship between an image and a container is similar to the relationship between a **class and an object** in object-oriented programming, or between a **recipe and a meal**:

- The **image** is the recipe — static, reusable, shareable.
- The **container** is the meal — the live, running thing created from that recipe.

You can create many containers from the same image, each running independently.

### Key characteristics of containers

- **Isolated**: Each container has its own filesystem, process space, and network interface. Containers don't interfere with each other or with the host.
- **Ephemeral by default**: When a container stops and is removed, any changes made inside it (files created, data written) are lost — unless you use **volumes** (persistent storage) to save data outside the container.
- **Lightweight**: A container is just a process (or group of processes) on the host. Starting one takes milliseconds.

### Container lifecycle

```
Image ──► docker run ──► Container (running)
                              │
                         docker stop
                              │
                         Container (stopped) ──► docker start ──► (running again)
                              │
                         docker rm
                              │
                            (gone)
```

---

## 9. Running Containers

The most fundamental command is `docker run`. It creates a container from an image and starts it.

### Basic syntax

```bash
docker run [OPTIONS] IMAGE [COMMAND]
```

### Your first container

```bash
# Run an interactive Python shell inside a container
docker run -it python:3.11-slim python
```

- `-it`: `-i` keeps stdin open (interactive), `-t` allocates a terminal. Together they give you an interactive shell.
- `python:3.11-slim`: the image to use.
- `python`: the command to run inside the container.

You're now inside a Python interpreter running in a container. Type `exit()` to leave. The container stops.

### Run a one-off command

```bash
# Print the Python version from inside a container
docker run --rm python:3.11-slim python --version
# Python 3.11.x
```

- `--rm`: automatically removes the container after it exits (keeps things clean).

### Run a container in the background (detached mode)

```bash
# Start a PostgreSQL database server in the background
docker run -d \
  --name my-postgres \
  -e POSTGRES_PASSWORD=secret \
  -p 5432:5432 \
  postgres:16
```

- `-d`: detached mode — runs in the background, returns the container ID.
- `--name my-postgres`: gives the container a readable name.
- `-e POSTGRES_PASSWORD=secret`: sets an environment variable inside the container.
- `-p 5432:5432`: maps port 5432 on your host to port 5432 in the container (`host:container`). Without this, the container's port is not accessible from your machine.

### Mount a local directory (bind mount)

By default, containers are isolated from your filesystem. To share files between your machine and a container:

```bash
docker run --rm \
  -v /path/on/your/machine:/path/inside/container \
  python:3.11-slim \
  python /path/inside/container/script.py
```

- `-v` (or `--volume`): mounts a directory from your host into the container.

For example, to run a local Python script without installing anything:

```bash
docker run --rm \
  -v $(pwd):/app \
  -w /app \
  python:3.11-slim \
  python train.py
```

- `$(pwd)`: your current directory on the host.
- `/app`: where it appears inside the container.
- `-w /app`: sets the working directory inside the container.

---

## 10. Essential Docker Commands

Here's a practical reference of the commands you'll use most often, organized by what they operate on.

### Images

```bash
# Search for an image on Docker Hub
docker search pandas

# Pull an image
docker pull python:3.11-slim

# List all local images
docker images

# Remove an image
docker rmi python:3.11-slim

# Remove all unused images (cleanup)
docker image prune
```

### Containers

```bash
# Run a container (creates + starts)
docker run [options] IMAGE

# List running containers
docker ps

# List all containers (including stopped ones)
docker ps -a

# Stop a running container
docker stop <container_name_or_id>

# Start a stopped container
docker start <container_name_or_id>

# Remove a stopped container
docker rm <container_name_or_id>

# Remove all stopped containers
docker container prune

# View logs from a container
docker logs <container_name_or_id>

# Follow logs in real time
docker logs -f <container_name_or_id>

# Execute a command inside a running container
docker exec -it <container_name_or_id> bash

# Copy a file from a container to your host
docker cp <container_id>:/path/in/container ./local/path
```

### Building images

```bash
# Build an image from a Dockerfile in the current directory
docker build -t my-image:1.0 .

# Build with a specific Dockerfile
docker build -f Dockerfile.prod -t my-image:prod .

# View build history (layers) of an image
docker history my-image:1.0
```

### System and cleanup

```bash
# Show disk usage (images, containers, volumes)
docker system df

# Remove everything unused (images, containers, networks, build cache)
docker system prune

# Remove everything including unused volumes
docker system prune --volumes

# Show resource usage of running containers (like htop for Docker)
docker stats
```

### Inspect and debug

```bash
# Inspect all metadata of a container or image
docker inspect <name_or_id>

# See the processes running inside a container
docker top <container_name>
```

---

## 11. Creating Your Own Image (Dockerfile)

A **Dockerfile** is a plain text file with step-by-step instructions for building a Docker image. Think of it as a recipe that Docker follows, line by line, to assemble your image.

### Dockerfile structure

Each line in a Dockerfile starts with an **instruction** (keyword) followed by arguments.

```dockerfile
# Comments start with #

FROM python:3.11-slim          # Start from this base image

WORKDIR /app                   # Set the working directory inside the container

COPY requirements.txt .        # Copy a file from your machine into the image

RUN uv pip install --no-cache-dir -r requirements.txt  # Run a shell command

COPY . .                       # Copy the rest of your project files

ENV MODEL_PATH=/app/model.pkl  # Set an environment variable

EXPOSE 8080                    # Document that the app uses port 8080 (informational)

CMD ["python", "train.py"]     # Default command to run when container starts
```

### The most important instructions

| Instruction | Purpose |
|------------|---------|
| `FROM` | Sets the base image. Every Dockerfile starts with this. |
| `WORKDIR` | Sets the working directory for subsequent instructions. Created if it doesn't exist. |
| `COPY` | Copies files/directories from your host into the image. |
| `RUN` | Executes a shell command during the image build (e.g., `uv pip install`). |
| `ENV` | Sets environment variables available inside the container. |
| `EXPOSE` | Documents which port the app listens on (does not actually publish the port). |
| `CMD` | The default command that runs when a container starts. Can be overridden. |
| `ENTRYPOINT` | Like `CMD`, but harder to override — used when the container should always run a specific executable. |
| `ARG` | Build-time variable (available only during `docker build`, not at runtime). |

### CMD vs ENTRYPOINT

```dockerfile
# CMD: fully replaceable default command
CMD ["python", "train.py"]
# docker run myimage python predict.py  ← overrides CMD

# ENTRYPOINT: the container always runs this executable
ENTRYPOINT ["python"]
CMD ["train.py"]
# docker run myimage predict.py  ← CMD becomes the argument to the entrypoint
```

### Layer caching best practice

Since Docker caches each layer, order your instructions to maximize cache reuse:

```dockerfile
# Good: copy requirements FIRST, install, THEN copy code
# If only code changes, the uv pip install layer is served from cache

COPY requirements.txt .
RUN uv pip install -r requirements.txt
COPY . .                        # ← code changes only invalidate this layer and below
```

```dockerfile
# Bad: copying everything first means any code change re-runs uv pip install

COPY . .
RUN uv pip install -r requirements.txt  # ← runs every time, even for small code changes
```

### .dockerignore

Just like `.gitignore`, a `.dockerignore` file tells Docker which files to exclude from the build context (what gets sent to Docker when you run `docker build`). This keeps your images lean and prevents secrets from being included:

```
# .dockerignore
__pycache__/
*.pyc
.env
.git/
data/raw/
*.csv
*.pkl
notebooks/
```

---

## 12. Example: Packaging a Python Regression Pipeline

Let's walk through a complete, realistic example: packaging a scikit-learn linear regression training pipeline into a Docker image.

### Project structure

```
regression-pipeline/
├── train.py
├── requirements.txt
├── Dockerfile
└── .dockerignore
```

### train.py

```python
import pickle
import numpy as np
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score

# Generate synthetic data (replace with your real data loading logic)
np.random.seed(42)
X = np.random.rand(200, 3)
y = 3 * X[:, 0] + 1.5 * X[:, 1] - 2 * X[:, 2] + np.random.randn(200) * 0.1

# Split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train
model = LinearRegression()
model.fit(X_train, y_train)

# Evaluate
y_pred = model.predict(X_test)
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
print(f"MSE:  {mse:.4f}")
print(f"R²:   {r2:.4f}")

# Save model
with open("model.pkl", "wb") as f:
    pickle.dump(model, f)

print("Model saved to model.pkl")
```

### requirements.txt

```
numpy==1.26.4
scikit-learn==1.4.2
```

### Dockerfile

```dockerfile
# Start from an official, minimal Python image
FROM python:3.11-slim

# Set the working directory inside the container
WORKDIR /app

# Copy requirements first to leverage layer caching
COPY requirements.txt .

# Install dependencies (no cache saves space in the image)
RUN uv pip install --no-cache-dir -r requirements.txt

# Copy the rest of the project
COPY . .

# Default command: run the training script
CMD ["python", "train.py"]
```

### .dockerignore

```
__pycache__/
*.pyc
.env
*.pkl
.git/
```

### Build the image

```bash
# From inside the regression-pipeline/ directory:
docker build -t regression-pipeline:1.0 .
```

You'll see Docker execute each instruction, with output like:

```
[+] Building 24.3s
 => [1/5] FROM python:3.11-slim
 => [2/5] WORKDIR /app
 => [3/5] COPY requirements.txt .
 => [4/5] RUN uv pip install --no-cache-dir -r requirements.txt
 => [5/5] COPY . .
 => exporting to image
```

### Run the container

```bash
docker run --rm regression-pipeline:1.0
```

Expected output:

```
MSE:  0.0103
R²:   0.9983
Model saved to model.pkl
```

### Save the trained model to your host machine

The `model.pkl` is written inside the container and lost when it exits. To persist it, mount a local directory:

```bash
mkdir -p ./output

docker run --rm \
  -v $(pwd)/output:/app/output \
  regression-pipeline:1.0 \
  python -c "
import pickle, numpy as np
from sklearn.linear_model import LinearRegression

X = np.random.rand(200, 3)
y = 3*X[:,0] + 1.5*X[:,1] - 2*X[:,2]
model = LinearRegression().fit(X, y)

with open('/app/output/model.pkl', 'wb') as f:
    pickle.dump(model, f)
print('Saved to /app/output/model.pkl')
"
```

Or better — update `train.py` to write to `output/model.pkl` and mount `./output` as the output directory.

### Rebuild and run interactively

While developing, you might want to explore the container:

```bash
docker run --rm -it \
  -v $(pwd):/app \
  regression-pipeline:1.0 \
  bash

# Now you're inside the container:
# root@abc123:/app# python train.py
# root@abc123:/app# ls
```

This mounts your local code live into the container — changes you make on your machine appear immediately inside.

---

## 13. Image Versioning and Tagging

When you build an image, you give it a **tag** — a label that typically encodes a version. This is how you track changes to your image over time and avoid confusion between "the version that worked" and "the version I just broke."

### Tag syntax

```
[registry/][username/]image-name:tag

python:3.11-slim          ← official image, tag = 3.11-slim
myusername/myapp:2.1.0    ← Docker Hub image
gcr.io/myproject/model:v3 ← Google Container Registry
```

If you don't specify a tag, Docker uses `latest` by default. **Avoid relying on `latest`** in production — it's ambiguous and can change unexpectedly.

### Tagging during build

```bash
# Single tag
docker build -t regression-pipeline:1.0 .

# Multiple tags in one build
docker build \
  -t regression-pipeline:1.0 \
  -t regression-pipeline:latest \
  .
```

### Tagging an existing image

```bash
# Create a new tag pointing to the same image
docker tag regression-pipeline:1.0 regression-pipeline:stable
```

### Semantic versioning convention

A common approach for ML/data science images:

```bash
docker build -t myorg/regression:1.0.0 .      # major.minor.patch
docker build -t myorg/regression:1.0.0-gpu .  # variant suffix
docker build -t myorg/regression:20240301 .   # date-based tag
```

### Pushing to Docker Hub

```bash
# Log in to Docker Hub
docker login

# Tag your image with your Docker Hub username
docker tag regression-pipeline:1.0 yourusername/regression-pipeline:1.0

# Push to Docker Hub
docker push yourusername/regression-pipeline:1.0
```

Once pushed, anyone can pull and run your image:

```bash
docker pull yourusername/regression-pipeline:1.0
docker run --rm yourusername/regression-pipeline:1.0
```

### A practical versioning workflow

```bash
# During development
docker build -t regression-pipeline:dev .

# When a version is ready
docker build -t regression-pipeline:1.1.0 .
docker tag regression-pipeline:1.1.0 regression-pipeline:latest

# If you need to roll back
docker run regression-pipeline:1.0.0  # ← use a specific version, not latest
```

---

## 14. The Docker Workflow in Data Science

Now let's zoom out and see how Docker fits into the full lifecycle of a data science project — from local experimentation to production deployment.

### The typical pain points Docker solves

| Pain point | Without Docker | With Docker |
|-----------|---------------|------------|
| Reproducing someone else's analysis | "Works on my machine" | `docker pull` and run |
| Moving from dev to production | Manual environment setup, version mismatches | Same image runs everywhere |
| Running experiments in parallel | Python environment conflicts | Each experiment in its own container |
| Collaborating with ML engineers | Long setup docs, environment drift | `Dockerfile` is the source of truth |
| Scheduling jobs on a cluster | Dependency management hell | Submit a container |

### The workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                        LOCAL DEVELOPMENT                        │
│                                                                 │
│  1. Write code + Dockerfile                                     │
│  2. docker build -t mymodel:dev .                               │
│  3. docker run --rm -v $(pwd):/app mymodel:dev python train.py  │
│  4. Iterate until satisfied                                     │
└───────────────────────────┬─────────────────────────────────────┘
                            │  docker push
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                     IMAGE REGISTRY                              │
│             (Docker Hub / ECR / GCR / GHCR)                    │
│                                                                 │
│   myorg/regression-pipeline:1.0.0                              │
└──────┬───────────────────────────────────────────┬─────────────┘
       │  docker pull                              │  docker pull
       ▼                                           ▼
┌──────────────┐                        ┌──────────────────────────┐
│  CI/CD       │                        │  PRODUCTION / CLOUD      │
│  Pipeline    │                        │                          │
│              │                        │  - AWS ECS / Fargate     │
│  Run tests   │                        │  - Google Cloud Run      │
│  inside      │                        │  - Kubernetes            │
│  container   │                        │  - Azure Container Apps  │
└──────────────┘                        └──────────────────────────┘
```

### Concrete data science scenarios

#### 1. Reproducible training runs

Package your entire training environment so anyone (including your future self) can reproduce the exact same results:

```bash
# Six months later, reproduce an old experiment
docker run --rm \
  -v $(pwd)/data:/app/data \
  -v $(pwd)/results:/app/results \
  myorg/regression-pipeline:1.0.0 \
  python train.py --config experiments/exp_001.yaml
```

#### 2. Running experiments in parallel

Each experiment gets its own container, avoiding package conflicts:

```bash
# Train with different hyperparameters simultaneously
docker run -d --name exp-lr-01 myorg/regression:dev python train.py --lr 0.01
docker run -d --name exp-lr-02 myorg/regression:dev python train.py --lr 0.001
docker run -d --name exp-lr-03 myorg/regression:dev python train.py --lr 0.0001

# Monitor all three
docker logs -f exp-lr-01
docker logs -f exp-lr-02
```

#### 3. Serving a model as an API

Once you're happy with training, package the model server the same way:

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN uv pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["uvicorn", "serve:app", "--host", "0.0.0.0", "--port", "8000"]
```

```bash
docker build -t regression-api:1.0 .
docker run -d -p 8000:8000 --name model-api regression-api:1.0

# The API is now accessible at http://localhost:8000
curl http://localhost:8000/predict -d '{"features": [0.5, 0.3, 0.2]}'
```

#### 4. Scheduled batch predictions

Run a prediction job nightly using a container, on a cloud scheduler (AWS EventBridge, GCP Cloud Scheduler):

```bash
# The cloud scheduler runs this command every night
docker run --rm \
  -e DB_CONNECTION_STRING=$DB_CONNECTION_STRING \
  -e OUTPUT_BUCKET=$OUTPUT_BUCKET \
  myorg/regression-pipeline:1.2.0 \
  python predict_batch.py
```

#### 5. CI/CD integration

In your GitHub Actions or GitLab CI pipeline, tests run inside a container — guaranteeing the same environment as production:

```yaml
# .github/workflows/train.yml
jobs:
  test-and-train:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build image
        run: docker build -t regression-pipeline:ci .
      - name: Run tests
        run: docker run --rm regression-pipeline:ci pytest tests/
      - name: Push to registry
        run: |
          docker tag regression-pipeline:ci myorg/regression-pipeline:${{ github.sha }}
          docker push myorg/regression-pipeline:${{ github.sha }}
```

### Summary: Why Docker matters for data scientists

1. **Reproducibility**: your model, training environment, and dependencies are all captured in a single artifact.
2. **Portability**: move from your laptop to a cloud GPU instance without any setup — just `docker pull`.
3. **Collaboration**: share a working environment with teammates or reviewers via an image, not a long README.
4. **Production readiness**: the same container you tested locally is the one that runs in production — no surprises.
5. **Isolation**: run multiple Python versions or conflicting library sets on the same machine without conflict.

---

## Quick Reference Card

```bash
# Build an image
docker build -t name:tag .

# Run a container
docker run --rm -it name:tag bash

# Run with port mapping
docker run -p host_port:container_port name:tag

# Run with volume
docker run -v /host/path:/container/path name:tag

# Run in background
docker run -d --name mycontainer name:tag

# List containers / images
docker ps -a
docker images

# Stop / remove
docker stop mycontainer
docker rm mycontainer
docker rmi name:tag

# Logs / exec
docker logs mycontainer
docker exec -it mycontainer bash

# Push to registry
docker tag name:tag username/name:tag
docker push username/name:tag

# Cleanup
docker system prune
```
