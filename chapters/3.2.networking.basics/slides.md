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

**NOTE**: You may be prompt by Firewall configuration to confirm the connectivity, click *Allow* to continue.

---

# Finding our web server port

* We will use `docker ps`:

    ```shell
    $ docker ps
    CONTAINER ID  ...  PORTS
    e40ffb406c9e  ...  0.0.0.0:32769->80/tcp, 0.0.0.0:32768->443/tcp
    ```

* The web server is running on ports 80 and 443 inside the container.
* Those ports are mapped to ports 32769 and 32768 on our Docker host.

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

---

# Why are we mapping ports?

* We are out of IPv4 addresses.
* Containers cannot have public IPv4 addresses.
* They have private addresses.
* Services have to be exposed port by port.
* Ports have to be mapped to avoid conflicts.

---

# Finding the web server port in a script

Parsing the output of `docker ps` would be painful.

* There is a command to help us:

    ```shell
    # docker port <containerID> 80
    $ docker port $(docker ps -lq) 80
    32769
    ```

---

# Manual allocation of port numbers

* If you want to set port numbers yourself, no problem:

    ```shell
    $ docker run -d -p 8000:80 nginx
    6fa50b3267e73b7a6cd7339d6216d276285ebf8c91e73f5c50ee24dbdfabde43
    $ docker run -d -p 8080:80 -p 8888:80 nginx
    a39f3047dd08692ba2724c238e0b5e7160418546ffc8f83091959c850739fb0d
    ```

* We are running two NGINX web servers.
* 1st is exposed on port 8000, 2nd is exposed on ports 8080 and 8888.

Note: the convention is `port-on-host:port-on-container`.

* Now let's test those:

    ```shell
    $ curl -s localhost:8000|grep h1
    <h1>Welcome to nginx!</h1>
    $ curl -s localhost:8080|grep h1
    <h1>Welcome to nginx!</h1>
    $ curl -s localhost:8888|grep h1
    <h1>Welcome to nginx!</h1>
    ```

---

# Plumbing containers into your infrastructure

There are many ways to integrate containers in your network.

* Start the container, letting Docker allocate a public port for it.
  <br/>Then retrieve that port number and feed it to your configuration.
* Pick a fixed port number in advance, when you generate your configuration.
  <br/>Then start your container by setting the port numbers manually.
* Use a network plugin, connecting your containers with e.g. VLANs, tunnels...
* Enable *Swarm Mode* to deploy across a cluster.
  <br/>The container will then be reachable through any node of the cluster.

---

# The different network drivers

A container can use one of the following drivers:

* `bridge` (default)
* `none`
* `host`
* `container`

The driver is selected with `docker run --net ...`.

---

# The default bridge

* By default, the container gets a virtual `eth0` interface.
  <br/>(In addition to its own private `lo` loopback interface.)
* That interface is provided by a `veth` pair.
* It is connected to the Docker bridge.
  <br/>(Named `docker0` by default; configurable with `--bridge`.)
* Addresses are allocated on a private, internal subnet.
  <br/>(Docker uses 172.17.0.0/16 by default; configurable with `--bip`.)
* Outbound traffic goes through an iptables MASQUERADE rule.
* Inbound traffic goes through an iptables DNAT rule.
* The container can have its own routes, iptables rules, etc.

---

# The null driver

* Container is started with `docker run --net none ...`
* It only gets the `lo` loopback interface. No `eth0`.
* It can't send or receive network traffic.
* Useful for isolated/untrusted workloads.

---

# The host driver

* Container is started with `docker run --net host ...`
* It sees (and can access) the network interfaces of the host.
* It can bind any address, any port (for ill and for good).
* Network traffic doesn't have to go through NAT, bridge, or veth.
* Performance = native!

Use cases:

* Performance sensitive applications (VOIP, gaming, streaming...)
* Peer discovery (e.g. Erlang port mapper, Raft, Serf...)

---

# The container driver

* Container is started with `docker run --net container:id ...`
* It re-uses the network stack of another container.
* It shares with this other container the same interfaces, IP address(es), routes, iptables rules, etc.
* Those containers can communicate over their `lo` interface.
  <br/>(i.e. one can bind to 127.0.0.1 and the others can connect to it.)

---

# Section summary

We've learned how to:

* Expose a network port.
* Manipulate container networking basics.
* Find a container's IP address.

In the next chapter, we will see how to connect
containers together without exposing their ports.
