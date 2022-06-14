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
* How to search and download images.
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

# A chicken-and-egg problem

* The only way to create an image is by "freezing" a container.
* The only way to create a container is by instanciating an image.
* Help!


---

# Creating the first images

There is a special empty image called `scratch`.

* It allows to *build from scratch*.

The `docker import` command loads a tarball into Docker.

* The imported tarball becomes a standalone image.
* That new image has a single layer.

Note: you will probably never have to do this yourself.


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

# Root namespace

The root namespace is for official images. They are put there by Docker Inc.,
but they are generally authored and maintained by third parties.

Those images include:

* Small, "swiss-army-knife" images like busybox.
* Distro images to be used as bases for your builds, like ubuntu, fedora...
* Ready-to-use components and services, like redis, postgresql...



---

# User namespace

The user namespace holds images for Docker Hub users and organizations.

* For example:

    ```shell
    jpetazzo/clock
    ```

* The Docker Hub user is:

    ```shell
    jpetazzo
    ```

* The image name is:

    ```shell
    clock
    ```

---

# Self-Hosted namespace

* This namespace holds images which are not hosted on Docker Hub, but on third party registries.

* They contain the hostname (or IP address), and optionally the port, of the registry server.

* For example:

    ```shell
    localhost:5000/wordpress
    ```

* `localhost:5000` is the host and port of the registry
* `wordpress` is the name of the image


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

# Searching for images

* We cannot list *all* images on a remote registry, but
we can search for a specific keyword:

    ```shell
    $ docker search zookeeper
    NAME                  DESCRIPTION                 STARS  OFFICIAL  AUTOMATED
    jplock/zookeeper      Builds a docker image ...   103              [OK]
    mesoscloud/zookeeper  ZooKeeper                   42               [OK]
    springxd/zookeeper    A Docker image that ca...   5                [OK]
    elevy/zookeeper       ZooKeeper configured t...   3                [OK]
    ```

* "Stars" indicate the popularity of the image.
* "Official" images are those in the root namespace.
* "Automated" images are built automatically by the Docker Hub.
  <br/>(This means that their build recipe is always available.)


---


# Downloading images

There are two ways to download images.

* Explicitly, with `docker pull`.
* Implicitly, when executing `docker run` and the image is not found locally.


---


# Pulling an image

* To pull an image do:

    ```shell
    $ docker pull alpine:3.5
    3.5: Pulling from library/alpine
    0a8490d0dfd3: Pull complete
    Digest: sha256:dfbd4a3a8ebca874ebd2474f044a0b33600d4523d03b0df76e5c5986cb02d7e8
    Status: Downloaded newer image for alpine:3.5

* As seen previously, images are made up of layers.
* Docker has downloaded all the necessary layers.
* In this example, `:3.5` indicates which exact version of Alpine
  we would like. It is a *version tag*.
* Alpine is a really small Operating System (~2Mb!), perfect for containers


---

# Image and tags

* Images can have tags.
* Tags define image versions or variants.
* `docker pull ubuntu` will refer to `ubuntu:latest`.
* The `:latest` tag is generally updated often.


---

# When to (not) use tags

Dont specify tags:

* When doing rapid testing and prototyping.
* When experimenting.
* When you want the latest version.

Do specify tags:

* When recording a procedure into a script.
* When going to production.
* To ensure that the same version will be used everywhere.
* To ensure repeatability later.


---

# Section summary

We've learned how to:

* Understand images and layers.
* Understand Docker image namespacing.
* Search and download images.
