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

layout: cover
# persist drawings in exports and build
drawings:
  persist: true
---

## 2-DAY TRAINING MASTERING DOCKER

<img class="absolute top-20 left-110" src="/chapters/0.1.introduction/logo.png" />

---

# Dimitris Kapanidis

* **Docker BCN** Meetup organizer (1.827 members)
* **Kubernetes BCN** Meetup organizer (411 members)
* Member of **Docker Captains** and **Google Developer Experts**

<img class="absolute bottom-0 left-10 w-100" src="/chapters/0.1.introduction/badges.png" />

---

# Table of Contents

We'll start by learning how to **run** containers and **build** Images:

## Section 1

- Environment Preparation
- Running Docker
- Docker Images
- Build Images Interactively

## Laboratory

- Build a Cowsay Docker Image

---

# Table of Contents

We'll discuss about how to build reproducible images using the `Dockerfile` syntax:

## Section 2

- Build Docker Images with a Dockerfile
- CMD and Entrypoint
- Copying files during build
- Advanced Dockerfile Syntax

## Laboratory

- Build a Golang Docker Image

---

# Table of Contents

We'll learn about the **Networking** behind containers:

## Section 3

- Naming and Inspecting Containers
- Networking Basics
- Container Network Model
- Connecting Containers with Links

## Laboratory

- Run Webapp with Redis client

---

# Table of Contents

Lastly we'll learn how to work with Docker in our **Development** workflow.

## Section 4

- Local Development Workflow with Docker
- Working with Volumes
- Compose for Development Stacks

## Laboratory

- Build and Run Docker Voting Stack
