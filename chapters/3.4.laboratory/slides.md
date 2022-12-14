---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
layout: two-cols
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

# Build and Run Guestbook App

We want to build and run a Guestbook App that uses Redis as backend.

* Clone this git repo:

    ```shell
    git clone https://github.com/spiddy/training
    ```

::right::

**Steps**

* In directory `docker/101/guestbook-node` build the Dockerfile as `guestbook` image
* Create a network `guestbook`
* Create a `redis:alpine` instance in `guestbook` network
* Create a `guestbook` instance in `guestbook` network and expose port `3000`
* Don't use `docker-compose` for this example!
