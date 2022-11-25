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

In this section we'll see how to prepare the environment with Docker

---

# Docker Playground

* Go to https://labs.play-with-docker.com/
* Sign in with your Docker credentials
* Sign up if you don't already have a user

<img class="absolute top-40 left-100 w-60 center" src="/chapters/1.1.environment/engine.png" />

---

# Docker Playground

Once started click on `+ Add new Instance` button

* To verify installation

    ```shell
    $ docker --version
    Docker version 20.10.17, build 100c701
    ```

* Run your first container

    ```shell
    $ docker run hello-world
    ...
    Hello from Docker!
    ...
    ```

