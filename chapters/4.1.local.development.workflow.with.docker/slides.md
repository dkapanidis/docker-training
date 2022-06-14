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

# 4.1. Local Development Workflow with Docker

<img class="absolute top-30 left-60" src="/chapters/4.1.local.development.workflow.with.docker/construction.jpg" />

---

# 4.1. Local Development Workflow with Docker

At the end of this lesson, you will be able to:

* Share code between container and host.
* Use a simple local development workflow.

---

# Using a Docker container for local development

Never again:

- "Works on my machine"
- "Not the same version"
- "Missing dependency"

By using Docker containers, we will get a consistent development environment.

---

# Our "namer" application

* The code is available on https://github.com/jpetazzo/namer.
* The image jpetazzo/namer is automatically built by the Docker Hub.

* Let's run it with:

    ```shell
    $ docker run -dP jpetazzo/namer
    b398fa5d14191206bd6d5735f1e8db6b6204aedbdc78af2297ac23069c960f1f
    ```

* Check the port number with `docker ps` and open the application.

    ```shell
    $ docker ps -l
    CONTAINER ID        IMAGE          ... PORTS                     ...
    60f6c9042cad        jpetazzo/namer ... 0.0.0.0:32772->9292/tcp   ...
    ```

---

# Let's look at the code

* Let's download our application's source code.

    ```shell
    $ git clone https://github.com/jpetazzo/namer
    $ cd namer
    $ ls -1
    company_name_generator.rb
    config.ru
    docker-compose.yml
    Dockerfile
    Gemfile
    ```

---

# Where's my code?

* According to the Dockerfile, the code is copied into `/src` :

    ```docker
    FROM ruby
    MAINTAINER Education Team at Docker <education@docker.com>

    COPY . /src
    WORKDIR /src
    RUN bundler install

    CMD ["rackup", "--host", "0.0.0.0"]
    EXPOSE 9292
    ```

We want to make changes *inside the container* without rebuilding it each time. 

For that, we will use a *volume*.

---

# Our first volume

* First, make sure to download dependencies locally (we'll overwrite the one's in the container)

    ```shell
    $ docker build -t namer .
    ```

* We will tell Docker to map the current directory to `/src` in the container.

    ```shell
    $ docker run -d -v "$(pwd):/src" -p 9292:9292 namer
    ```

* The ``-d`` flag indicates that the container should run in detached mode (in the background).
* The ``-v`` flag provides volume mounting inside containers.
* The ``-p`` flag maps port ``9292`` inside the container to port ``80`` on the host.
* ``namer`` is the name of the image we will run. Since we built it right before, it will use the above.
* We don't need to give a command to run because the Dockerfile already specifies `rackup`.

---

# Mounting volumes inside containers

* The ``-v`` flag mounts a directory from your host into your Docker
container. The flag structure is:

    ```shell
    [host-path]:[container-path]:[rw|ro]
    ```

* If [host-path] or [container-path] doesn't exist it is created.
* You can control the write status of the volume with the ``ro`` and
  ``rw`` options.
* If you don't specify ``rw`` or ``ro``, it will be ``rw`` by default.

There will be a full chapter about volumes!

---

# Testing the development container

* Now let us see if our new container is running:

    ```shell
    $ docker ps
    CONTAINER ID  IMAGE   PORTS                NAMES
    045885b68bc5  trai... 0.0.0.0:9292->9292/tcp ...
    ```

---

# Viewing our application

* Now let's browse to our web application on:

    ```shell
    http://<yourHostIP>:9292
    ```

* We can see our company naming application. 

<img class="absolute top-70 left-60" src="/chapters/4.1.local.development.workflow.with.docker/webapp1.png" />

---

# Making a change to our application

* Our customer really doesn't like the color of our text. Let's change it.

    ```shell
    $ vi company_name_generator.rb
    ```

* And change

    ```css
    color: royalblue;
    ```

* To:

    ```css
    color: red;
    ```

---

# Refreshing our application

* Now let's refresh our browser:

    ```shell
    http://<yourHostIP>:9292
    ```

* We can see the updated color of our company naming application.

<img class="absolute top-70 left-60" src="/chapters/4.1.local.development.workflow.with.docker/webapp2.png" />

---

# Improving the workflow with Compose

* You can also start the container with the following command:

    ```shell
    $ docker-compose up -d
    ```

* This works thanks to the Compose file, `docker-compose.yml`:

  ```yaml
  version: '3'
  services:
    www:
      build: .
      volumes:
        - .:/src
      ports:
        - 9292:9292
    ```

---

# Why Compose?

* Specifying all those "docker run" parameters is tedious.
* And error-prone.
* We can "encode" those parameters in a "Compose file."
* When you see a `docker-compose.yml` file, you know that you can use `docker-compose up`.
* Compose can also deal with complex, multi-container apps.
  <br/>(More on this later.)

---

# Workflow explained

We can see a simple workflow:

1. Build an image containing our development environment.

    (Rails, Django...)

2. Start a container from that image.

    Use the ``-v`` flag to mount source code inside the container.

3. Edit source code outside the containers, using regular tools.

    (vim, emacs, textmate...)

4. Test application.

    (Some frameworks pick up changes automatically.

    Others require you to Ctrl-C + restart after each modification.)

5. Repeat last two steps until satisfied.

6. When done, commit+push source code changes.

    (You *are* using version control, right?)

---

# Stopping the container

* Now that we're done let's stop our container.

    ```shell
    $ docker stop $(docker ps -q --filter status=running)
    ```

* And remove it.

    ```shell
    $ docker rm $(docker ps -qa)
    ```

---

# Section summary

We've learned how to:

* Share code between container and host.
* Set our working directory.
* Use a simple local development workflow.

