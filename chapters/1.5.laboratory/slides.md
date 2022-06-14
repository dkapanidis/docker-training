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

### 1.5. Laboratory

Build a Cowsay Docker Image

* Create a container that shows a cow saying "Hello"

    ```shell
    $ docker run -it cowsay
    root@0d7cddc33f19:/# /usr/games/cowsay hello
     _______
    < Hello >
     -------
            \   ^__^
             \  (oo)\_______
                (__)\       )\/\
                    ||----w |
                    ||     ||
    ```

::right::

Remember
* You can use `ubuntu` as base image
* Do `apt-get update` to retrieve the repository index
* You can use `cowsay` package to draw the cow on screen (binary is `/usr/games/cowsay` once installed - not in PATH for some weird reason)
* Commit your container to a `cowsay` image
* Run a new instance to say Hello as the example above
