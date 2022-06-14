---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: true
---


# 1.1 Environment Preparation

<img class="absolute top-40 left-50" src="/chapters/1.1.environment/tools.jpg" />

---

# 1.1 Environment Preparation

In this section we'll see how to prepare the environment with the necessary tools

- Docker (a.k.a Docker Engine)
- Docker Compose
- Docker Machine

---

# Install Docker

<img class="absolute top-40 left-100 w-60 center" src="/chapters/1.1.environment/engine.png" />

---

# Install Docker

During this lesson you'll learn:

- How to install Docker
- When to use `sudo` when running Docker commands

---

# Install Docker

Docker is easy to install

It runs on:

- A variety of Linux Distributions
- OS X via a Virtual Machine
- Microsoft Windows via a Virtual Machine

TLDR; Guide:

- Google "Install Docker"
- Follow https://docs.docker.com/engine/installation steps for your distro

---

# Installing on OS X and Microsoft Windows

Docker doesn't run natively on OS X or Microsoft Windows.

There are three ways to get Docker on OS X or Windows:

- Using Docker Mac or Docker Windows (recommended);
- Using the Docker Toolbox (formerly recommended);
- Rolling your own with e.g. Parallels, VirtualBox, VMware...

---

# Running Docker on OS X and Windows

When you execute docker version from the terminal:

- the CLI connects to the Docker Engine over a standard socket,
- the Docker Engine is, in fact, running in a VM,
- ... but the CLI doesn't know or care about that,
- the CLI sends a request using the REST API,
- the Docker Engine in the VM processes the request,
- the CLI gets the response and displays it to you.

All communication with the Docker Engine happens over the API.

This will also allow to use remote Engines exactly as if they were local.

---

# Verify Docker Installation

* To verify installation

    ```shell
    $ docker --version
    Docker version 18.02.0-ce, build fc4de44
    ```

* Run your first container

    ```shell
    $ docker run hello-world
    ...
    Hello from Docker!
    ...
    ```

---

# The docker group

* Add the Docker group

    ```shell
    $ sudo groupadd docker
    ```

* Add ourselves to the group

    ```shell
    sudo gpasswd -a $USER docker
    ```

* Restart the Docker daemon

    ```shell
    $ sudo service docker restart
    ```

* Log out

    ```shell
    $ exit
    ```

---

# Install Docker Compose

Compose is a tool for defining and running multi-container Docker applications.

With Compose, you use a Compose file to configure your application’s services. Then, using a single command, you create and start all the services from your configuration.

<img class="absolute bottom-10 right-10 w-30" src="/chapters/1.1.environment/compose.png" />

---

# Install Docker Compose

On macOS and Windows, Compose is installed when you install the Docker for Mac, Docker for Windows, or Docker Toolbox.

* On Linux:

    ```shell
    $ curl -L "https://github.com/docker/compose/releases/download/1.11.1/docker-compose-$(uname -s)-$(uname -m)" -o /tmp/docker-compose
    $ chmod +x /tmp/docker-compose
    $ sudo cp /tmp/docker-compose /usr/local/bin/docker-compose
    ```

* To verify installation

    ```shell
    $ docker-compose --version
    docker-compose version 1.19.0, build 9e633ef
    ```

<img class="absolute bottom-10 right-10 w-30 opacity-30 -z-10" src="/chapters/1.1.environment/compose.png" />

---

# Install Docker Machine

You can use Docker Machine to:

- Provision and manage multiple remote Docker hosts
- Provision Swarm clusters

<img class="absolute bottom-10 right-10 w-30" src="/chapters/1.1.environment/machine.png" />

---

# Install Docker Machine

On macOS and Windows, Machine is installed when you install the Docker for Mac, Docker for Windows, or Docker Toolbox.

* On Linux:

    ```shell
    $ curl -L "https://github.com/docker/machine/releases/download/v0.9.0/docker-machine-$(uname -s)-$(uname -m)" -o /tmp/docker-machine
    $ chmod +x /tmp/docker-machine
    $ sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
    ```

* To verify installation

    ```shell
    $ docker-machine version
    docker-machine version 0.13.0, build 9ba6da9
    ```

<img class="absolute bottom-10 right-10 w-30 opacity-30 -z-10" src="/chapters/1.1.environment/machine.png" />

---

# 1.1 Environment Preparation

After this section we now have achieved the following:

- Install Docker Engine
- Install Docker Compose
- Install Docker Machine
- Run a hello-world container
