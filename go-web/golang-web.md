# Golang Web Application in a Container



## 1. Build a Static Executable

### Write hello-world source code in the host

create the following program, `helloWeb.go` in an empty directory, for example `go-web/hello`:

```
$ mkdir -p go-web/hello && cd go-web/hello
$ cat > helloWeb.go
package main

import "fmt"

func main(){
    fmt.Printf("hello, world\n")
}
<ctrl+d>
$ cat hello.go
```

### Build an executable

```
(image) --> [docker container run go build] --> executable
```
Run the container by combining several options:

1. mounting the directory where your source code is located onto the container's src directory:

    -v $(pwd):/go/src/hello

2. changing the working directory:

    cd /go/src/hello

3. and running the go build command with necessary flags 

    

```
$ docker container run --rm -it -v $(pwd):/go/src/hello go-img 
    # cd /go/src/hello
    # ls
    helloWeb.go
    # CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -tags netgo -ldflags '-w' .
    # ls
    hello   helloWeb.go
    # exit
$ ls
hello helloWeb.go
$
```

# 2. Create an image for hello-web

```
(Dockerfile and program) --> [Docker] --> image
```
Package your `hello` Linux executable in a Linux container image by creating a Dockerfile in an empty directory along with the `hello` program:

```
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
Now, build the image `hello-web:latest`:

```
$ docker image build -t hello-web:latest .
$ docker image ls
REPOSITORY   TAG     SIZE
hello-web    latest  5.86MB
. . .
$
```

# 3. Run the web application

Run your `hello-web` container:

```
$ docker container run --name web --rm -d hello-web
. . .
$ docker container ls
IMAGE     COMMAND   STATUS         NAMES
hello-web "/hello"  Up 27 seconds  web
```

Check with your web broweser

http://localhost:8080/hello

You should see:

```
hello, web!
```

Stop the container:

```
$ docker container stop web
$ docker container ls
$
``