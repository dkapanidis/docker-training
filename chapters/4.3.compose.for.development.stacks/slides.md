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

# Section 4.3 - Compose for Development Stacks

<img class="absolute top-40 left-80" src="/chapters/4.3.compose.for.development.stacks/fig.png" />

---

# Section 4.3 - Compose for Development Stacks

Dockerfiles are great to build a single container.

But when you want to start a complex stack made of multiple containers,
you need a different tool. This tool is Docker Compose.

In this lesson, you will use Compose to bootstrap a development environment.

---

# Working with Volumes

Docker volumes can be used to achieve many things, including:

* Bypassing the copy-on-write system to obtain native disk I/O performance.
* Bypassing copy-on-write to leave some files out of ``docker commit``.
* Sharing a directory between multiple containers.
* Sharing a directory between the host and a container.
* Sharing a *single file* between the host and a container.

---

# Volumes are special directories in a container

Volumes can be declared in two different ways.

* Within a ``Dockerfile``, with a ``VOLUME`` instruction.

    ```docker
    VOLUME /uploads
    ```

* On the command-line, with the ``-v`` flag for ``docker run``.

    ```shell
    $ docker run -d -v /uploads myapp
    ```

In both cases, ``/uploads`` (inside the container) will be a volume.

---

# Volumes exist independently of containers

If a container is stopped, its volumes still exist and are available.

* Since Docker 1.9, we can see all existing volumes and manipulate them:

    ```shell
    $ docker volume ls
    DRIVER              VOLUME NAME
    local               5b0b65e4316da67c2d471086640e6005ca2264f3...
    local               pgdata-prod
    local               pgdata-dev
    local               13b59c9936d78d109d094693446e174e5480d973...
    ```

Some of those volume names were explicit (pgdata-prod, pgdata-dev).

The others (the hex IDs) were generated automatically by Docker.

---

# Named volumes (since Engine 1.9)

* We can now create and manipulate volumes as first-class concepts.
* Volumes can be created without a container, then used in multiple containers.

* Let's create a volume directly.

    ```shell
    $ docker volume create --name=website
    website
    ```

Volumes are not anchored to a specific path.

---

# Using our named volumes

* Volumes are used with the `-v` option.
* When a host path does not contain a /, it is considered to be a volume name.

* Let's start a web server using the two previous volumes.

    ```shell
    $ docker run -d -p 8888:80 \
             -v website:/usr/share/nginx/html \
             -v logs:/var/log/nginx \
             nginx
    ```

* Check that it's running correctly:

    ```shell
    $ curl -s localhost:8888
    <!DOCTYPE html>
    ...
    <h1>Welcome to nginx!</h1>
    ...
    ```

---

# Using a volume in another container

* We will make changes to the volume from another container.
* In this example, we will run a text editor in the other container, but this could be a FTP server, a WebDAV server, a Git receiver...

* Let's start another container using the `website` volume.

    ```shell
    $ docker run -v website:/website -w /website -ti alpine vi index.html
    ```

Make changes, save, and exit.

* Then check your changes on nginx:

    ```shell
    $ curl -s localhost:8888
    ```

---

# Managing volumes explicitly

In some cases, you want a specific directory on the host to be mapped
inside the container:

* You want to manage storage and snapshots yourself.

    (With LVM, or a SAN, or ZFS, or anything else!)

* You have a separate disk with better performance (SSD) or resiliency (EBS)
  than the system disk, and you want to put important data on that disk.

* You want to share your source directory between your host (where the
  source gets edited) and the container (where it is compiled or executed).

* Wait, we already met the last use-case in our example development workflow!
Nice.

    ```shell
    $ docker run -d -v /path/on/the/host:/path/in/container image ...
    ```

---

# What happens when you remove containers with volumes?

* With Engine versions prior 1.9, volumes would be *orphaned* when the last container referencing them is destroyed.
* Orphaned volumes are not deleted, but you cannot access them.

    (Unless you do some serious archaeology in `/var/lib/docker`.)

* Since Engine 1.9, orphaned volumes can be listed with `docker volume ls` and mounted to containers with `-v`.

Ultimately, _you_ are the one responsible for logging,
monitoring, and backup of your volumes.

---

# Sharing a single file between the host and a container

The same ``-v`` flag can be used to share a single file.

* One of the most interesting examples is to share the Docker control socket.

    ```shell
    $ docker run -it -v /var/run/docker.sock:/var/run/docker.sock docker sh
    ```

Warning: when using such mounts, the container gains root-like access to the host.
It can potentially do bad things.

---

# Volume plugins

You can install plugins to manage volumes backed by particular storage systems,
or providing extra features. For instance:

* [dvol](https://github.com/ClusterHQ/dvol) - allows to commit/branch/rollback volumes;
* [Flocker](https://clusterhq.com/flocker/introduction/), [REX-Ray](https://github.com/emccode/rexray) - create and manage volumes backed by an enterprise storage system (e.g. SAN or NAS), or by cloud block stores (e.g. EBS);
* [Blockbridge](http://www.blockbridge.com/), [Portworx](http://portworx.com/) - provide distributed block store for containers;
* and much more!

---

# Section summary

We've learned how to:

* Create and manage volumes.
* Share volumes across containers.
* Share a host directory with one or many containers.

