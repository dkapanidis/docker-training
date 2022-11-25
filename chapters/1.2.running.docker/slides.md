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


# 1.2 Running Docker

<img class="absolute top-30 left-30" src="/chapters/1.2.running.docker/running.png" />

---

# 1.2 Running Docker

At the end of this lesson you'll have learned

- The Docker architecture
- How to run Docker containers

---

# Docker Architecture

Docker is a client-server application.

- **The Docker Engine** (or "daemon")

    Receives and processes incoming Docker API requests.

- **The Docker client**

    Talks to the Docker daemon via the Docker API.

    We'll use mostly the CLI embedded within the docker binary.

- **Docker Hub Registry**

    Collection of public images.

    The Docker daemon talks to it via the registry API.

---

# Hello World

* In your Docker environment, just run the following command:

    ```shell
    $ docker run alpine echo hello world
    hello world
    ```

Notes:

- If `alpine` image was not used before, it will be downloaded automatically
- During presentation we'll execute commands directly to see output on screen


---

# That was our first container!

* We used one of the smallest, simplest images available: `alpine`.
* `alpine` is typically used in embedded systems (phones, routers...)
* We ran a single process and echo'ed `hello world`.

---

# A more useful container

* Let's run a more exciting container:

    ```shell
    $ docker run -it ubuntu
    root@4f9263507465:/#
    ```

* This is a brand new container.
* It runs a bare-bones, no-frills `ubuntu` system.
* `-it` is shorthand for `-i -t`.

  * `-i` tells Docker to connect us to the container's stdin.
  * `-t` tells Docker that we want a pseudo-terminal.

---

# Do something in our container

* Try to run `figlet` in our container.

    ```shell
    root@4f9263507465:/# figlet hello
    bash: figlet: command not found
    ```

Alright, we need to install it.

---

# Install a package in our container

* We want `figlet`, so let's install it:

    ```shell
    root@4f9263507465:/# apt-get update
    ...
    Fetched 1514 kB in 14s (103 kB/s)
    Reading package lists... Done
    root@4f9263507465:/# apt-get install figlet
    Reading package lists... Done
    ...
    ```

* One minute later, `figlet` is installed!

    ```shell
    # figlet hello
     _          _ _
    | |__   ___| | | ___
    | '_ \ / _ \ | |/ _ \
    | | | |  __/ | | (_) |
    |_| |_|\___|_|_|\___/
    ```

---

# Exiting our container

* Just exit the shell, like you would usually do (E.g. with `^D` or `exit`)

    ```shell
    root@4f9263507465:/# exit
    ```

* Our container is now in a *stopped* state.
* It still exists on disk, but all compute resources have been freed up.

---

# Starting another container

* What if we start a new container, and try to run `figlet` again?

    ```shell
    $ docker run -it ubuntu
    root@31b883f8d6c8:/# figlet
    bash: figlet: command not found
    ```

* We started a *brand new container*.
* The basic Ubuntu image was used, and `figlet` is not here.
* We will see in the next chapters how to bake a custom image with `figlet`.

---

# A non-interactive container

* We will run a small custom container. This container just displays the time every second.

    ```shell
    $ docker run jpetazzo/clock
    Fri Feb 20 00:28:53 UTC 2015
    Fri Feb 20 00:28:54 UTC 2015
    Fri Feb 20 00:28:55 UTC 2015
    ...
    ```

* This container will run forever.
* To stop it, press `^C`.
* Docker has automatically downloaded the image `jpetazzo/clock`.
* This image is a user image, created by `jpetazzo`.
* We will hear more about user images (and other types of images) later.

---

# Run a container in the background

* Containers can be started in the background, with the `-d` flag (daemon mode):

    ```shell
    $ docker run -d jpetazzo/clock
    47d677dcfba4277c6cc68fcaa51f932b544cab1a187c853b7d0caf4e8debe5ad
    ```

* We don't see the output of the container.
* But don't worry: Docker collects that output and logs it!
* Docker gives us the ID of the container.

---

# List running containers

* How can we check that our container is still running?

* With `docker ps`, just like the UNIX `ps` command, lists running processes.

    ```shell
    $ docker ps
    CONTAINER ID  IMAGE           ...  CREATED        STATUS        ...
    47d677dcfba4  jpetazzo/clock  ...  2 minutes ago  Up 2 minutes  ...
    ```

Docker tells us:

* The (truncated) ID of our container.
* The image used to start the container.
* That our container has been running (`Up`) for a couple of minutes.
* Other information (COMMAND, PORTS, NAMES) that we will explain later.

---

# Starting more containers

* Let's start two more containers.

    ```shell
    $ docker run -d jpetazzo/clock
    57ad9bdfc06bb4407c47220cf59ce21585dce9a1298d7a67488359aeaea8ae2a
    $ docker run -d jpetazzo/clock
    068cc994ffd0190bbe025ba74e4c0771a5d8f14734af772ddee8dc1aaf20567d
    ```

* Check that `docker ps` correctly reports all 3 containers.

    ```shell
    $ docker ps
    CONTAINER ID  IMAGE           ...  CREATED        STATUS        ...
    47d677dcfba4  jpetazzo/clock  ...  2 minutes ago  Up 2 minutes  ...
    57ad9bdfc06b  jpetazzo/clock  ...  8 seconds ago  Up 6 seconds  ...
    068cc994ffd0  jpetazzo/clock  ...  8 seconds ago  Up 6 seconds  ...
    ```

---

# View the logs of a container

* We told you that Docker was logging the container output. Let's see that now.

    ```shell
    $ docker logs -f $(docker ps -lq)
    Sat Feb 25 14:55:46 UTC 2022
    Sat Feb 25 14:55:47 UTC 2022
    ...
    ```

* We specified a *prefix* of the full container ID.
* You can, of course, specify the full ID.
* The `logs` command will output the *entire* logs of the container.
  <br/>(Sometimes, that will be too much. Let's see how to address that.)
* with flag `-f`, we can follow the logs of our container.
* Use `^C` to exit.

---

# Stop our container

There are two ways we can terminate our detached container.

* Killing it using the `docker kill` command.
* Stopping it using the `docker stop` command.

The first one stops the container immediately, by using the
`KILL` signal.

The second one is more graceful. It sends a `TERM` signal,
and after 10 seconds, if the container has not stopped, it
sends `KILL.`

Reminder: the `KILL` signal cannot be intercepted, and will
forcibly terminate the container.