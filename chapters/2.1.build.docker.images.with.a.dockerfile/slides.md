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

# 2.1. Build Docker Images with a Dockerfile

<img class="absolute top-30 left-50" src="/chapters/2.1.build.docker.images.with.a.dockerfile/containers.jpg" />

---

# Dockerfile overview

* A `Dockerfile` is a build recipe for a Docker image.
* It contains a series of instructions telling Docker how an image is constructed.
* The `docker build` command builds an image from a ``Dockerfile``.

---

# Writing our first `Dockerfile`

Our Dockerfile must be in a **new, empty directory**.

1. Create a directory to hold our ``Dockerfile``.

    ```shell
    $ mkdir -p training/figlet/
    ```

2. Create a ``Dockerfile`` inside this directory.

    ```shell
    $ cd training/figlet/
    $ vim Dockerfile
    ```

Of course, you can use any other editor of your choice.

---

# Type this into our Dockerfile...

* Write the following:

    ```docker
    FROM ubuntu
    RUN apt-get update
    RUN apt-get install figlet
    ```

* `FROM` indicates the base image for our build.
* Each `RUN` line will be executed by Docker during the build.
* Our `RUN` commands **must be non-interactive.**
  <br/>(No input can be provided to Docker during the build.)
* In many cases, we will add the `-y` flag to `apt-get`.

---

# Build it!

* Save our file, then execute:

    ```shell
    $ docker build -t figlet .
    ```

* `-t` indicates the tag to apply to the image.
* `.` indicates the location of the *build context*.
  <br/>(We will talk more about the build context later;
  but to keep things simple: this is the directory where
  our Dockerfile is located.)

---

# What happens when we build the image?

* The output of `docker build` looks like this:

    ```shell
    $ cd training/figlet/
    $ docker build -t figlet .
    Sending build context to Docker daemon 2.048 kB
    Sending build context to Docker daemon
    Step 1/3 : FROM ubuntu
     ---> f49eec89601e
    Step 2/3 : RUN apt-get update
     ---> Running in d5167bcc1c45
     ---> 3efa17a47e0c
    Removing intermediate container d5167bcc1c45
    Step 3/3 : RUN apt-get install figlet
     ---> Running in 442cd64b1a43
     ---> bbf166d0be0b
    Removing intermediate container 442cd64b1a43
    Successfully built bbf166d0be0b
    ```

* The output of the `RUN` commands has been omitted.
* Let's explain what this output means.

---

# The caching system

If you run the same build again, it will be instantaneous.

Why?

* After each build step, Docker takes a snapshot of the resulting image.
* Before executing a step, Docker checks if it has already built the
  same sequence.
* Docker uses the exact strings defined in your Dockerfile, so:

  * `RUN apt-get install figlet cowsay `
    <br/> is different from
    <br/> `RUN apt-get install cowsay figlet`
  * `RUN apt-get update` is not re-executed when the mirrors are updated

You can force a rebuild with `docker build --no-cache ...`.

---

# Running the image

* Let's run it:

    ```shell
    $ docker run -ti figlet
    root@91f3c974c9a1:/# figlet hello
     _          _ _
    | |__   ___| | | ___
    | '_ \ / _ \ | |/ _ \
    | | | |  __/ | | (_) |
    |_| |_|\___|_|_|\___/
    ```

* Sweet is the taste of success!

