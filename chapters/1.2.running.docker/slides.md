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

**NOTE**: If you're using Windows, open a `PowerShell` but not `PowerShell ISE`, it seems to have some issues capturing input.

---

# Do something in our container

* Try to run `figlet` in our container.

    ```shell
    root@4f9263507465:/# figlet hello
    bash: figlet: command not found
    ```

Alright, we need to install it.

---

# An observation

* Let's check how many packages are installed here.

    ```shell
    root@4f9263507465:/# dpkg -l | wc -l
    189
    ```

* `dpkg -l` lists the packages installed in our container
* `wc -l` counts them
* If you have a Debian or Ubuntu machine, you can run the same command
  and compare the results.

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

# Two useful flags for `docker ps`

* To see only the last container that was started:

    ```shell
    $ docker ps -l
    CONTAINER ID  IMAGE           ...  CREATED        STATUS        ...
    068cc994ffd0  jpetazzo/clock  ...  2 minutes ago  Up 2 minutes  ...
    ```

* To see only the ID of containers:

    ```shell
    $ docker ps -q
    068cc994ffd0
    57ad9bdfc06b
    47d677dcfba4
    ```

* Combine those flags to see only the ID of the last container started!

    ```shell
    $ docker ps -lq
    068cc994ffd0
    ```

---

# View the logs of a container

* We told you that Docker was logging the container output. Let's see that now.

    ```shell
    $ docker logs $(docker ps -lq)
    Sat Feb 25 14:55:46 UTC 2017
    Sat Feb 25 14:55:47 UTC 2017
    ...
    ```

* We specified a *prefix* of the full container ID.
* You can, of course, specify the full ID.
* The `logs` command will output the *entire* logs of the container.
  <br/>(Sometimes, that will be too much. Let's see how to address that.)

---

# View only the tail of the logs

* To avoid being spammed with eleventy pages of output,
we can use the `--tail` option:

    ```shell
    $ docker logs --tail 10 $(docker ps -lq)
    Sat Feb 25 14:57:46 UTC 2017
    Sat Feb 25 14:57:47 UTC 2017
    Sat Feb 25 14:57:48 UTC 2017
    Sat Feb 25 14:57:49 UTC 2017
    Sat Feb 25 14:57:50 UTC 2017
    Sat Feb 25 14:57:51 UTC 2017
    Sat Feb 25 14:57:52 UTC 2017
    Sat Feb 25 14:57:53 UTC 2017
    Sat Feb 25 14:57:54 UTC 2017
    Sat Feb 25 14:57:55 UTC 2017
    ```

* The parameter is the number of lines that we want to see.

---

# Follow the logs in real time

* Just like with the standard UNIX command `tail -f`, we can
follow the logs of our container:

    ```shell
    $ docker logs --tail 1 --follow $(docker ps -lq)
    Sat Feb 25 14:57:46 UTC 2017
    Sat Feb 25 14:57:47 UTC 2017
    ^C
    ```

* This will display the last line in the log file.
* Then, it will continue to display the logs in real time.
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

---

# Stopping our containers

* Let's stop one of those containers:

    ```shell
    $ docker stop $(docker ps -lq)
    47d6
    ```

This will take 10 seconds:

* Docker sends the TERM signal;
* the container doesn't react to this signal
  (it's a simple Shell script with no special
  signal handling);
* 10 seconds later, since the container is still
  running, Docker sends the KILL signal;
* this terminates the container.

---

# Killing the remaining containers

* Let's be less patient with the two other containers:

    ```shell
    $ docker kill $(docker ps -q)
    13b94211a272
    dc67beda1cea
    ```

The `stop` and `kill` commands can take multiple container IDs.

Those containers will be terminated immediately (without
the 10 seconds delay).

* Let's check that our containers don't show up anymore:

    ```shell
    $ docker ps
    CONTAINER ID  IMAGE           ...  CREATED        STATUS        ...
    ```

---

# List stopped containers

* We can also see all containers, with the `-a` (`--all`) option.

    ```shell
    $ docker ps -a
    CONTAINER ID  IMAGE           ...  STATUS
    068cc994ffd0  jpetazzo/clock  ...  Exited (137) 3 min. ago
    57ad9bdfc06b  jpetazzo/clock  ...  Exited (137) 3 min. ago
    47d677dcfba4  jpetazzo/clock  ...  Exited (137) 3 min. ago
    5c1dfd4d81f1  jpetazzo/clock  ...  Exited (0) 40 min. ago
    ```

* To cleanup the stopped containers, with the `-f` (`--filter`) option.

    ```shell
    #  We filter containers with status 'created' or 'exited'
    $ docker rm $(docker ps -aq -f status=created -f status=exited)
    ba14ef59aaad
    d55fb08eab4c
    9b423d1e6c35
    ```

---

# Background and foreground

The distinction between foreground and background containers is arbitrary.

From Docker's point of view, all containers are the same.

All containers run the same way, whether there is a client attached to them or not.

It is always possible to detach from a container, and to reattach to a container.

Analogy: attaching to a container is like plugging a keyboard and screen to a physical server.

---

# Detaching from a container

* If you have started an *interactive* container (with option `-it`),
  <br/>you can detach from it.
* The "detach" sequence is `^P^Q`.
* Otherwise you can detach by killing the Docker client.
  <br/>(But not by hitting `^C`, as this would deliver `SIGINT`
  to the container.)

What does `-it` stand for?

* `-t` means "allocate a terminal."
* `-i` means "connect stdin to the terminal."

---

# Detaching from non-interactive containers

* **Warning:** if the container was started without `-it`...

  * You won't be able to detach with `^P^Q`.
  * If you hit `^C`, the signal will be proxied to the container.

* Remember: you can always detach by killing the Docker client.

---

# Checking container output

* Use `docker attach` if you intend to send input to the container.
* If you just want to see the output of a container, use `docker logs`.

    ```shell
    $ docker logs --tail 1 --follow <containerID>
    ```

---

# Restarting a container

* When a container has exited, it is in stopped state. It can then be restarted with the `start` command.

     ```shell
     $ docker start <yourContainerID>
     ```

The container will be restarted using the same options you launched it
with.

* You can re-attach to it if you want to interact with it:

    ```shell
    $ docker attach <yourContainerID>
    ```

Use `docker ps -a` to identify the container ID of a previous `jpetazzo/clock` container and try those commands.
