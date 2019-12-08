# First images and containers

1. [Image with empty filesystem](https://github.com/irisqaz/practice-docker/blob/master/README.md#image-with-empty-filesystem)

2. [Image with one program](https://github.com/irisqaz/practice-docker/blob/master/README.md#image-with-one-program)

## 1. Image with empty filesystem



### Package your empty filesystem

```
(empty tar file) --> [Docker] --> image
```

Create the following tar file `empty.tar` with will be empty.  This will be your packaged empty filesystem

`ex1/empty.tar`

```shell
$ mkdir ex1
$ cd ex1
$ touch empty.tar
$ ls
empty.tar
```

### Create the Docker image

Import `empty.tar` to an image in Docker and name it `empty:latest` :

```shell
$ docker import empty.tar empty:latest
sha256: . . .
$
```
```
(tar file) --> [Docker] --> image
```

List your images and find your new image `empty`.  It will have a size of `0B`--it's empty.

```shell
$ docker image ls
REPOSITORY  TAG       IMAGE ID       CREATED        SIZE
empty       latest    42b085b8925e   6 seconds ago  0B
$
```

finally,

### Run the container

```
(image) --> [Docker] --> container + command
```

The attempt will fail, because running a container means running
a program, an which we don't yet have, as its filesystem is empty

```shell
$ docker container run --rm -t empty:latest
```

We get the error:

```shell
docker: Error response from daemon: No command specified.
See 'docker run --help'.
$
```

hmmm . . . Let's try the `ls` utility program:

```shell
$ docker container run --rm -t empty:latest ls
```

We get the error:

```shell
docker: Error response from daemon: OCI runtime create failed:
container_linux.go:346: starting container process caused "exec:
\"ls\": executable file not found in $PATH": unknown.
```

Then we need to put the `ls` program in our container . . .

## Image with one program

```
(tar file with program) --> [Docker] --> image
```

Let's say that you want to solve the following problem:

```
What is the content of the empty filesystem?  or 
what does the Linux Kernel in the images look like?
```

All we need is  to be able to run the utility `ls`.

Solution:
`run the ls command`

But to run the `ls` utility in the container, the utility has to be in the image.  Let's get it from another public Docker image: `busybox`.  Note: if you are already doing the exercises in Linux, the solution is simpler, but let's go over the use of 3rd patry images to solve some of our problems.



```shell
$ 
```

