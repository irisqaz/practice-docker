# Using the Golang Containers



## 1. Build and Run hello-world 


### Pull the golang image

```
(image name) --> [docker pull] --> image
```

Run the following command to pull a specific version of golang:

```
$ docker image pull golang:1.13.5
$ docker image ls
```

Create a new tag to refer to this same image to make it easier to type the name of the image:

```
$ docker image tag golang:1.13.5 go-img
$ docker image ls
```

### Run some golang containers

Explore the image contents by running a container:

```
$ docker container run --rm -it go-img
# 
```

Find the locations for the working directory and $GOPATH:

```
# pwd
/go
# echo $GOPATH
/go
# go version
go version go1.13.5 linux/amd64
# ls
bin src
# ls src/
# exit
```

### Write hello-world source code in the host

create the following program, `hello.go` in an empty directory, for example `hello`:

```
$ mkdir hello; cd hello
$ cat > hello.go
package main

import "fmt"

func main(){
    fmt.Printf("hello, world\n")
}
<ctrl+d>
$ cat hello.go
```

### Build and run in the container

```
(image) --> [docker container run] --> container + command
```
Run the container by mounting the directory where your source code is located onto the container's src directory:
```
$ docker container run --rm -it -v $(pwd):/go/src/hello go-img go run src/hello/hello.go
hello, world!
$
```

### Build an executable

create the executable `hello` from the `hello.go` source file:

```
$ docker container run --rm -it -v $(pwd):/go/src/hello -w /go/src/hello go-img go build
$ ls
hello hello.go
```

If your host is not Linux, you cannot run the `hello` executable:

```
$ ./hello
bash: ./hello: cannot execute binary file
```

# 2. Create an image for hello-world

```
(Dockerfile and program) --> [Docker] --> image
```
Package your `hello` Linux executable in a Linux container image by creating a Dockerfile in an empty directory along with the `hello` program:

```
$ mkdir bin
$ cp hello bin/
$ cd bin
$ cat > Dockerfile
FROM scratch
COPY hello /
CMD ["/hello"]
<ctrl+d>
$ cat Dockerfile
FROM scratch
COPY hello /
CMD ["/hello"]
```
Now, build the image `hello:latest`:

```
$ docker image build -t hello:latest .
$ docker image ls
REPOSITORY TAG     SIZE
hello      latest  2.03MB
. . .
$
```

Run your `hello` container:

```
$ docker container run --rm -it hello
hello, world!
$
```
