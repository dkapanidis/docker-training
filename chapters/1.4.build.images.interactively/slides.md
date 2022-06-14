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

# Section 1.4 - Build Images Interactively

<img class="absolute top-30 left-50" src="/chapters/1.4.build.images.interactively/interactive.jpg" />

---

# Section 1.4 - Build Images Interactively

In this lesson, we will create our first container image.

It will be a basic distribution image, but we will pre-install
the package `figlet`.

We will: 

* Create a container from a base image.
* Install software manually in the container, and turn it
  into a new image.
* Learn about new commands: `docker commit`, `docker tag`, and `docker diff`.

---

# Building Images Interactively

As we have seen, the images on the Docker Hub are sometimes very basic.

How do we want to construct our own images?

As an example, we will build an image that has `figlet`.

First, we will do it manually with `docker commit`.

Then, in an upcoming chapter, we will use a `Dockerfile` and `docker build`.

---

# Building from a base

Our base will be the `ubuntu` image.

---

# Create a new container and make some changes

* Start an Ubuntu container:

    ```shell
    $ docker run -it ubuntu
    root@dee8255c3bc5:#/
    ```

Run the command `apt-get update` to refresh the list of packages available to install.

* Then run the command `apt-get install figlet` to install the program we are interested in.

    ```shell
    root@dee8255c3bc5:#/ apt-get update && apt-get install figlet
    .... OUTPUT OF APT-GET COMMANDS ....
    ```

---

# Inspect the changes

* Type ``exit`` at the container prompt to leave the interactive session.

* Now let's run `docker diff` to see the difference between the base image
and our container.

    ```shell
    $ docker diff $(docker ps -lq)
    C /root
    A /root/.bash_history
    C /tmp
    C /usr
    C /usr/bin
    A /usr/bin/figlet
    ...
    ```

---

# Commit and run your image

* The `docker commit` command will create a new layer with those changes and a new image using this new layer.

    ```shell
    $ docker commit $(docker ps -lq)
    sha256:11700b224dbf51cbabadbb09268810911551490322ef4cc4320...
    ```

* The output of the ``docker commit`` command will be the ID for your newly created image.

* We can run this image:

    ```shell
    $ docker run -it <IMAGE_ID>
    root@fcfb62f0bfde:/# figlet hello
     _          _ _       
    | |__   ___| | | ___  
    | '_ \ / _ \ | |/ _ \ 
    | | | |  __/ | | (_) |
    |_| |_|\___|_|_|\___/ 
    ```

---

# Tagging images

* Referring to an image by its ID is not convenient. Let's tag it instead.

* We can use the `tag` command:

    ```shell
    $ docker tag <IMAGE_ID> figlet
    ```

* But we can also specify the tag as an extra argument to `commit`:

    ```shell
    $ docker commit $(docker ps -lq) figlet
    sha256:333e419eec70d65db983375c583360f0613125673a07f3da35f...
    ```

* And then run it using its tag:

    ```shell
    $ docker run -it figlet
    root@0d7cddc33f19:/# figlet hello
     _          _ _
    | |__   ___| | | ___
    | '_ \ / _ \ | |/ _ \
    | | | |  __/ | | (_) |
    |_| |_|\___|_|_|\___/
    ```

---

# What's next?

Manual process = bad.

Automated process = good.

In the next chapter, we will learn how to automate the build
process by writing a `Dockerfile`.
