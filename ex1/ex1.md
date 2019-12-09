# First images and containers

1. [Image with empty filesystem](https://github.com/irisqaz/practice-docker/blob/master/ex1/ex1.md#1-image-with-empty-filesystem)

2. [Image with one program](https://github.com/irisqaz/practice-docker/blob/master/ex1/ex1.md#2-image-with-one-program)

## 1. Image with empty filesystem



### Package an empty filesystem

```
(empty tar file) --> [Docker] --> image
```

Create the following tar file `empty.tar` wich will be empty.  This will be your packaged empty filesystem

`ex1/empty.tar`

```shell
$ mkdir ex1; cd ex1
$ touch empty.tar
$ ls
empty.tar
```

### Create the Docker image

Import `empty.tar` to an image in Docker and name it `empty:latest` :

```
(tar file) --> [docker image import] --> image

$ docker image import empty.tar empty:latest
sha256: . . .
$
```

List your images and find your new image `empty`.  It will have a size of `0B`--it's empty.

```
$ docker image ls
REPOSITORY  TAG       IMAGE ID       CREATED        SIZE
empty       latest    42b085b8925e   6 seconds ago  0B
$
```

finally,

### Run the container

```
(image) --> [docker container run] --> container + command
```

The attempt will fail, because running a container means running
a program, which we don't yet have, as its filesystem is empty

```shell
$ docker container run --rm empty:latest
```

We get the error:

```
docker: Error response from daemon: No command specified.
See 'docker run --help'.
$
```

**Note**: The option `--rm` removes the container after the `run` command.

hmmm . . . Let's try the `ls` utility program:

```shell
$ docker container run --rm empty:latest ls
```

We get the error:

```
"exec:\"ls\": executable file not found in $PATH": unknown.
```

Then we need to put the `ls` program in our container . . .

## 2. Image with one program

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

**copy the `ls` utility from busybox image to your local directory**

```
(local dir) --> [docker container run busybox] -->|
(local dir) <---------------------- cp /bin/ls <--|
```

Before, pull the image

```shell
$ docker image pull busybox
```

Now, run the `cp` command to copy the `ls` program to our current directory.

```shell
$ docker container run --rm -v "$(pwd)":/tempDir busybox cp /bin/ls /tempDir
$
$ ls
empty.tar ls
$
```

**Note**: The `-v` mounts a local directory, current directory `$(pwd)` onto the container in the specified directory, /tempDir 


My host computer is Mac OS; so I cannot execute this `ls` program, but it should run if the host is Linux.

```shell
$ ./ls
-bash: ./ls: cannot execute binary file
$
```

Now, that we have a program, let's package it into a new image:


```
$ tar -cvf ls.tar ls
a ls
$ ls
empty.tar   ls   ls.tar
$
```

```
(tar file with program) --> [docker image import] --> image

$ docker image import ls.tar ls:latest
sha256: . . .
$
```

Check the new image:

```shell
$ docker image ls
REPOSITORY  TAG       IMAGE ID       CREATED        SIZE
ls          latest    ...            ...            1.13MB
empty       latest    ...            ...            0B
busybox     latest    ...            ...            1.22MB
$
```


Now, let's run the `ls` container with the command `/ls`:

```
(image) --> [docker container run] --> container + command

$ docker container run --rm -t ls:latest /ls
dev   etc   ls    proc  sys
$
```

**Note**: The `-t` option attaches a terminal display to the container.  Without it, the output would be as follows:

```shell
$ docker container run --rm ls:latest /ls
dev
etc
ls
proc
sys
$
```

Besides our program `ls`, we can see the content of what appears to be the minimal filesystem for the Linux Kernel:

```
dev   etc   proc  sys
```

The containers `run` from this image must provide provide `/ls` program at the end of the `run` declaration; otherwise you'll get an error.

But it allows you to use the proper arguments to list the contents of the given directory:

```
$ docker container run --rm -t ls:latest /ls dev
console  fd       mqueue   ptmx     random   stderr   stdout   urandom
core     full     null     pts      shm      stdin    tty      zero
$
```

