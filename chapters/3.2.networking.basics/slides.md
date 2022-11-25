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

# Section 3.2 - Networking Basics

<img class="absolute top-30 left-40" src="/chapters/3.2.networking.basics/networking.png" />

---

# A simple, static web server

* Run the Docker Hub image `nginx`, which contains a basic web server:

    ```shell
    $ docker run -d -P nginx
    66b1ce719198711292c8f34f84a7b68c3876cf9f67015e752b94e189d35a204e
    ```

* Docker will download the image from the Docker Hub.
* `-d` tells Docker to run the image in the background.
* `-P` tells Docker to make this service reachable from other computers.
  <br/>(`-P` is the short version of `--publish-all`.)

But, how do we connect to our web server now?

---

# Finding our web server port

* We will use `docker ps`:

    ```shell
    $ docker ps
    CONTAINER ID  ...  PORTS
    e40ffb406c9e  ...  0.0.0.0:32769->80/tcp
    ```

* The web server is running on port 80 inside the container.
* The port is mapped to 32769 on our Docker host.

We will explain the whys and hows of this port mapping.

But first, let's make sure that everything works properly.

---

# Connecting to our web server (GUI)

Point your browser to the IP address of your Docker host, on the port
shown by `docker ps` for container port 80.

<img class="absolute top-40 left-40" src="/chapters/3.2.networking.basics/web.png" />

---

# Connecting to our web server (CLI)

You can also use `curl` directly from the Docker host.

* Make sure to use the right port number if it is different from the example below:

    ```shell
    $ curl localhost:32769
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    ...
    ```
