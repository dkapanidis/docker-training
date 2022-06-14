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

# Section 3.1 - Naming and Inspecting Containers

<img class="absolute top-30 left-60" src="/chapters/3.1.naming.and.inspecting.containers/containermarkings.jpg" />

---

# Section 3.1 - Naming and Inspecting Containers

In this lesson, we will learn about an important
Docker concept: container *naming*.

Naming allows us to:

* Reference easily a container.
* Ensure unicity of a specific container.

We will also see the `inspect` command, which gives a lot of details about a container.

---

# More images

* We will need some more images today:

    ```shell
    docker pull node:7.7.0-alpine
    docker pull postgres:9.4
    ```
