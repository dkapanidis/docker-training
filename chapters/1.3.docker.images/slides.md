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

# Section 1.3 - Docker Images

<img class="absolute top-30 left-50" src="/chapters/1.3.docker.images/images.png" />

---

# Section 1.3 - Docker Images

In this lesson, we will explain:

* What is an image.
* What is a layer.
* The various image namespaces.
* Image tags and when to use them.


---

# What is an Image?

* An image is a collection of files + some meta data.
  <br/>(Technically: those files form the root filesystem of a container.)
* Images are made of *layers*, conceptually stacked on top of each other.
* Each layer can add, change, and remove files.
* Images can share layers to optimize disk usage, transfer times, and memory use.

* Example:
  * CentOS
  * JRE
  * Tomcat
  * Dependencies
  * Application JAR
  * Configuration

---

# Image vs Container

* An image is a read-only filesystem.
* A container is an encapsulated set of processes running in a
  read-write copy of that filesystem.
* To optimize container boot time, *copy-on-write* is used
  instead of regular copy.
* `docker run` starts a container from a given image.

Let's give a couple of metaphors to illustrate those concepts.

---

# Image as stencils

Images are like templates or stencils that you can create containers from.

<img class="absolute top-50 left-90" src="/chapters/1.3.docker.images/stenciling-wall.jpg" />


---

# Object-oriented programming

* Images are conceptually similar to *classes*.
* Layers are conceptually similar to *inheritance*.
* Containers are conceptually similar to *instances*.

---

# Wait a minute...

If an image is read-only, how do we change it?

* We don't.
* We create a new container from that image.
* Then we make changes to that container.
* When we are satisfied with those changes, we transform them into a new layer.
* A new image is created by stacking the new layer on top of the old image.

---

# Creating other images

`docker commit`

* Saves all the changes made to a container into a new layer.
* Creates a new image (effectively a copy of the container).

`docker build`

* Performs a repeatable build sequence.
* This is the preferred method!

We will explain both methods in a moment.


---

# Images namespaces

There are three namespaces:

* Official images

    e.g. `ubuntu`, `busybox` ...

* User (and organizations) images

    e.g. `jpetazzo/clock`

* Self-hosted images

    e.g. `registry.example.com:5000/my-private/image`

Let's explain each of them.


---

# How do you store and manage images?

Images can be stored:

* On your Docker host.
* In a Docker registry.

You can use the Docker client to download (pull) or upload (push) images.

To be more accurate: you can use the Docker client to tell a Docker server
to push and pull images to and from a registry.


---

# Showing current images

* Let's look at what images are on our host now.

    ```shell
    $ docker images
    REPOSITORY       TAG       IMAGE ID       CREATED         SIZE
    fedora           latest    ddd5c9c1d0f2   3 days ago      204.7 MB
    centos           latest    d0e7f81ca65c   3 days ago      196.6 MB
    ubuntu           latest    07c86167cdc4   4 days ago      188 MB
    redis            latest    4f5f397d4b7c   5 days ago      177.6 MB
    postgres         latest    afe2b5e1859b   5 days ago      264.5 MB
    alpine           latest    70c557e50ed6   5 days ago      4.798 MB
    debian           latest    f50f9524513f   6 days ago      125.1 MB
    busybox          latest    3240943c9ea3   2 weeks ago     1.114 MB
    training/namer   latest    902673acc741   9 months ago    289.3 MB
    jpetazzo/clock   latest    12068b93616f   12 months ago   2.433 MB
    ```

---

# Image and tags

* Images can have tags.
* Tags define image versions or variants.
* `docker pull ubuntu` will refer to `ubuntu:latest`.
* The `:latest` tag is generally updated often.
