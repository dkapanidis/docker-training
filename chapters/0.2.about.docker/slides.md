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

# About Docker

<img class="absolute top-30 left-40" src="/chapters/0.2.about.docker/who.png" />

---

# Elevator pitch

## (for your manager, boss...)

---

# OK... Why the buzz around containers?

* The software industry has changed.
* Before:
  * monolithic applications
  * long development cycles
  * single environment
  * slowly scaling up
* Now:
  * decoupled services
  * fast, iterative improvements
  * multiple environments
  * quickly scaling out

---

# Deployment becomes very complex

* Many different stacks:
  * languages
  * frameworks
  * databases
* Many different targets:
  * individual development environments
  * pre-production, QA, staging...
  * production: on prem, cloud, hybrid

---

# The deployment problem

<img class="absolute top-23 left-10" src="/chapters/0.2.about.docker/problem.png" />

---

# The Matrix from Hell

<img class="absolute top-23 left-10" src="/chapters/0.2.about.docker/matrix.png" />

---

# An inspiration and some ancient history!

<img class="absolute top-23 left-10" src="/chapters/0.2.about.docker/history.png" />

---

# Intermodal shipping containers

<img class="absolute top-23 left-10" src="/chapters/0.2.about.docker/shipping.png" />

---

# This spawned a Shipping Container Ecosystem!


<img class="absolute top-23 left-10" src="/chapters/0.2.about.docker/shipeco.png" />

---

# A shipping container system for applications

<img class="absolute top-23 left-10" src="/chapters/0.2.about.docker/appcont.png" />

---

# Eliminate the matrix from Hell

<img class="absolute top-23 left-10" src="/chapters/0.2.about.docker/elimatrix.png" />

---

# Results

* Dev-to-prod reduced from 9 months to 15 minutes (ING)

* Continuous integration job time reduced by more than 60% (BBC)

* Dev-to-prod reduced from weeks to minutes (GILT)

---

# Elevator pitch

## (for your fellow devs and ops)

---

# Escape dependency hell

1. Write installation instructions into an `INSTALL.txt` file
2. Using this file, write an `install.sh` script that works *for you*
3. Turn this file into a `Dockerfile`, test it on your machine
4. If the Dockerfile builds on your machine, it will build *anywhere*
5. Rejoice as you escape dependency hell and "works on my machine"

Never again "worked in dev - ops problem now!"

---

# On-board developers and contributors rapidly

1. Write Dockerfiles for your application components
2. Use pre-made images from the Docker Hub (mysql, redis...)
3. Describe your stack with a Compose file
4. On-board somebody with two commands:

    ```shell
    git clone ...
    docker-compose up
    ```

Also works to create dev, integration, QA environments in minutes!

---

# Implement reliable CI easily

1. Build test environment with a Dockerfile or Compose file
2. For each test run, stage up a new container or stack
3. Each run is now in a clean environment
4. No pollution from previous tests

Way faster and cheaper than creating VMs each time!

