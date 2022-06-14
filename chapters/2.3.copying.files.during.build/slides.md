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

# Section 2.4 - Copying files during build

<img class="absolute top-20 left-0" src="/chapters/2.3.copying.files.during.build/copying.jpg" />

---

# Section 2.4 - Copying files during build

So far, we have installed things in our container images
by downloading packages.

We can also copy files from the *build context* to the
container that we are building.

Remember: the *build context* is the directory containing
the Dockerfile.

In this chapter, we will learn a new Dockerfile keyword: `COPY`.

---

# Build some C code

We want to build a container that compiles a basic "Hello world" program in C.

* Here is the program, `hello.c`:

    ```c
    int main () {
      puts("Hello, world!");
      return 0;
    }
    ```

Let's create a new directory, and put this file in there.

Then we will write the Dockerfile.

---

# The Dockerfile

* On Debian and Ubuntu, the package `build-essential` will get us a compiler.

* When installing it, don't forget to specify the `-y` flag, otherwise the build will fail (since the build cannot be interactive).

* Then we will use `COPY` to place the source file into the container.

    ```docker
    FROM ubuntu
    RUN apt-get update
    RUN apt-get install -y build-essential
    COPY hello.c /
    RUN make hello
    CMD /hello
    ```

Create this Dockerfile.

---

# Testing our C program

* Create `hello.c` and `Dockerfile` in the same direcotry.
* Run `docker build -t hello .` in this directory.

    ```shell
    $ cd training/hello-c
    $ docker build -t hello .
    Successfully built f7ca4c22771f
    ```

* Run `docker run hello`, you should see `Hello, world!`.

    ```shell
    $ docker run hello
    Hello, world!
    ```

Success!

---

# `COPY` and the build cache

* Run the build again.
* Now, modify `hello.c` and run the build again.
* Docker can cache steps involving `COPY`.
* Those steps will not be executed again if the files haven't been changed.

---

# Details

* You can `COPY` whole directories recursively.
* Older Dockerfiles also have the `ADD` instruction.
  <br/>It is similar but can automatically extract archives.
* If we really wanted to compile C code in a compiler, we would:

  * Place it in a different directory, with the `WORKDIR` instruction.
  * Even better, use the `gcc` official image.
