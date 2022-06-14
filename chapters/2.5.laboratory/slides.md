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

### 2.5. Laboratory

* Build a container with a Golang hello example

    ```shell
    $ docker run hello-go
    Hello world from Go!
    ```

* Here is a Hello world in Go (`hello.go`)

    ```go
    package main
    import "fmt"
    func main() {
        fmt.Printf("Hello world from Go!\n")
    }
    ```

::right::

Remember

* You can use as parent image `golang:1.8.0-alpine`
* Your code needs to be on `/go/src/` directory
* To compile the code you need to run `go build SOURCE_CODE`
* Your final binary is located at `/go/hello`
* Build your image with name `hello-go`
* Run a container to print the output
