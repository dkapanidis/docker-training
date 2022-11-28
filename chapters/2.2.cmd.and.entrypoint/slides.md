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

# 2.2. CMD and Entrypoint

<img class="absolute top-30 left-0" src="/chapters/2.2.cmd.and.entrypoint/entrypoint.jpg" />

---

# 2.2. CMD and Entrypoint

In this lesson, we will learn about two important Dockerfile commands:

* `CMD`
* `ENTRYPOINT`

Those commands allow us to set the default command to run in a container

---

# Defining a default command

When people run our container, we want to greet them with a nice hello message, and using a custom font.

* For that, we will execute:

    ```shell
    $ figlet -f script hello
     _          _   _
    | |        | | | |
    | |     _  | | | |  __
    |/ \   |/  |/  |/  /  \_
    |   |_/|__/|__/|__/\__/
    ```

* `-f script` tells figlet to use a fancy font.
* `hello` is the message that we want it to display.

---

# Adding `CMD` to our Dockerfile

* Our new Dockerfile will look like this:

    ```docker
    FROM ubuntu
    RUN apt-get update
    RUN ["apt-get", "install", "figlet"]
    CMD figlet -f script hello
    ```

* `CMD` defines a default command to run when none is given.
* It can appear at any point in the file.
* Each `CMD` will replace and override the previous one.
* As a result, while you can have multiple `CMD` lines, it is useless.

---

# Build and test our image

* Let's build it:

    ```shell
    $ cd training/figlet-cmd/
    $ docker build -t figlet .
    ...
    Successfully built 042dff3b4a8d
    ```

* And run it:

    ```shell
    $ docker run figlet
     _          _   _
    | |        | | | |
    | |     _  | | | |  __
    |/ \   |/  |/  |/  /  \_
    |   |_/|__/|__/|__/\__/
    ```

---

# Overriding `CMD`

* If we want to get a shell into our container (instead of running `figlet`), we just have to specify a different program to run:

    ```shell
    $ docker run -it figlet bash
    root@7ac86a641116:/#
    ```

* We specified `bash`.
* It replaced the value of `CMD`.

---

# Using `ENTRYPOINT`

We want to be able to specify a different message on the command line,
while retaining `figlet` and some default parameters.

* In other words, we would like to be able to do this:

    ```shell
    $ docker run figlet salut
               _
              | |
     ,   __,  | |       _|_
    / \_/  |  |/  |   |  |
     \/ \_/|_/|__/ \_/|_/|_/
     ```

We will use the `ENTRYPOINT` verb in Dockerfile.


---

# Adding `ENTRYPOINT` to our Dockerfile

* Our new Dockerfile will look like this:

    ```docker
    FROM ubuntu
    RUN apt-get update
    RUN ["apt-get", "install", "figlet"]
    ENTRYPOINT ["figlet", "-f", "script"]
    ```

* `ENTRYPOINT` defines a base command (and its parameters) for the container.
* The command line arguments are appended to those parameters.
* Like `CMD`, `ENTRYPOINT` can appear anywhere, and replaces the previous value.

Why did we use JSON syntax for our `ENTRYPOINT`?

---

# Implications of JSON vs string syntax

* When CMD or ENTRYPOINT use string syntax, they get wrapped in `sh -c`.
* To avoid this wrapping, you must use JSON syntax.

* What if we used `ENTRYPOINT` with string syntax?

    ```shell
    $ docker run figlet salut
    ```

* This would run the following command in the `figlet` image:

    ```shell
    sh -c "figlet -f script" salut
    ```

---

# Build and test our image

* Let's build it:

    ```shell
    $ cd training/figlet-entrypoint/
    $ docker build -t figlet .
    ...
    Successfully built 36f588918d73
    ```

* And run it:

    ```shell
    $ docker run figlet salut
               _
              | |
     ,   __,  | |       _|_
    / \_/  |  |/  |   |  |
     \/ \_/|_/|__/ \_/|_/|_/
     ```

Great success!

---

# Using `CMD` and `ENTRYPOINT` together

What if we want to define a default message for our container?

Then we will use `ENTRYPOINT` and `CMD` together.

* `ENTRYPOINT` will define the base command for our container.
* `CMD` will define the default parameter(s) for this command.
* They *both* have to use JSON syntax.

---

# `CMD` and `ENTRYPOINT` together

* Our new Dockerfile will look like this:

    ```docker
    FROM ubuntu
    RUN apt-get update
    RUN ["apt-get", "install", "figlet"]
    ENTRYPOINT ["figlet", "-f", "script"]
    CMD ["hello world"]
    ```

* `ENTRYPOINT` defines a base command (and its parameters) for the container.
* If we don't specify extra command-line arguments when starting the container,
  the value of `CMD` is appended.
* Otherwise, our extra command-line arguments are used instead of `CMD`.

---

# Build and test our image

* Let's build it:

    ```shell
    $ cd training/figlet-cmd-entrypoint/
    $ docker build -t figlet .
    ...
    Successfully built 6e0b6a048a07
    ```

* And run it:

    ```shell
    $ docker run figlet
     _          _   _                             _
    | |        | | | |                           | |    |
    | |     _  | | | |  __             __   ,_   | |  __|
    |/ \   |/  |/  |/  /  \_  |  |  |_/  \_/  |  |/  /  |
    |   |_/|__/|__/|__/\__/    \/ \/  \__/    |_/|__/\_/|_/

    $ docker run figlet hola mundo
     _           _
    | |         | |                                      |
    | |     __  | |  __,     _  _  _           _  _    __|   __
    |/ \   /  \_|/  /  |    / |/ |/ |  |   |  / |/ |  /  |  /  \_
    |   |_/\__/ |__/\_/|_/    |  |  |_/ \_/|_/  |  |_/\_/|_/\__/
    ```

---

# Overriding `ENTRYPOINT`

What if we want to run a shell in our container?

We cannot just do `docker run figlet bash` because
that would just tell figlet to display the word "bash."

* We use the `--entrypoint` parameter:

    ```shell
    $ docker run -it --entrypoint bash figlet
    root@6027e44e2955:/#
    ```
