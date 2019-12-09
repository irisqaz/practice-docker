# Dockerfile instead of tar file

Repeat the exercises from ex1, but instead of packaging a filesystem in a tar file, use the Dockerfile directives to do the same.

1. [Image with empty filesystem](https://github.com/irisqaz/practice-docker/blob/master/ex2/ex2.md#1-image-with-empty-filesystem)

2. [Image with one program and CMD](https://github.com/irisqaz/practice-docker/blob/master/ex2/ex2.md#2-image-with-one-program-and-cmd)

3. [Image with one program and ENTRYPOINT](https://github.com/irisqaz/practice-docker/blob/master/ex2/ex2.md#3-image-with-one-program-and-entrypoint)

## 1. Image with empty filesystem 


### Package an empty filesystem

```
(empty Dockerfile) --> [Docker] --> image
```

Create the following `Dockerfile` file wich will contain the necessary directives but will not add any files or directories.  This will be your packaged empty filesystem

`ex2/Dockerfile`

```shell
$ mkdir ex2; cd ex2
$ touch Dockerfile
$ ls
Dockerfile
```

Copy and paste the following directives:

```Dockerfile
FROM scratch
CMD [""]
```

### Build the Docker image

Run the `build` Docker command to build an image from a Dockerfile.
**Note**: the last character `.` indicates the current directory; the `-t` option indicated the tag's name
```
(empty Dockerfile) --> [docker image build] --> image

$ docker image build -t empty:v1 .
. . .
Successfully tagged empty:v1
$
```

List your images and find your new image `empty:v1`.  It will have a size of `0B`--it's empty.

```
$ docker image ls
REPOSITORY  TAG   IMAGE ID       CREATED        SIZE
empty       v1    d31bd92a588b   6 seconds ago  0B
$
```

finally,

### Run the container

```
(image) --> [docker container run] --> container + command
```

The attempt will fail, because running a container means running
a program, which we don't yet have, as its filesystem is empty and the `CMD` directive was also empty.

```shell
$ docker container run --rm empty:v1
```

We get the error:

```
"exec: \"\": executable file not found in $PATH": unknown.
$
```

**Note**: The option `--rm` removes the container after the `run` command.

hmmm . . . Let's try the `ls` utility program:

```shell
$ docker container run --rm empty:latest ls
```

We get the error:

```
"exec: \"ls\": executable file not 
found in $PATH": unknown.
```

Then we need to put the `ls` program in our container . . .

## 2. Image with one program and CMD

```
(Dockerfile with program) --> [Docker] --> image
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

**copy the `ls` program from the previous exercise**

```
$ cp ../ex1/ls .
$ ls
Dockerfile   ls
$
```

Now, that we have a program, let's package it into a new image using directives in the Dockerfile.  Edit your Dockerfile with the following content:

```Dockerfile
FROM scratch
COPY ls /
CMD ["/ls"]
```

. . . and build the image
```
(Dockerfile with program) --> [docker image build] --> image

$ docker image build -t ls:v1 .
. . .
Successfully tagged ls:v1
$
```

Check the new image:

```
$ docker image ls
REPOSITORY  TAG       IMAGE ID       CREATED        SIZE
ls          v1        ...            ...            1.13MB
$
```


Now, let's run the `ls` container with and without the command `/ls` and some arguments:

```
(image) --> [docker container run] --> container + command

$ docker container run --rm -t ls:v1 /ls
dev   etc   ls    proc  sys
$ docker container run --rm -t ls:v1
dev   etc   ls    proc  sys
$  docker container run --rm -t ls:v1 /ls dev
console  fd       mqueue   ptmx     random   stderr   stdout   urandom
core     full     null     pts      shm      stdin    tty      zero
$
```

## 3. Image with one program and ENTRYPOINT

Use the following Dockerfile that replaces the CMD directive with the ENTRYPOINT directive:

```Dockerfile
FROM scratch
COPY ls /
ENTRYPOINT ["/ls"]
```

. . . and build the image
```
(Dockerfile with program) --> [docker image build] --> image

$ docker image build -t ls:v2 .
. . .
Successfully tagged ls:v2
$
```

Now, let's run the `ls` container with and without arguments:

```
 docker container run --rm -t ls:v2
dev   etc   ls    proc  sys
$ docker container run --rm -t ls:v2 -l
total 1112
drwxr-xr-x    5 0        0              360 Dec  9 02:35 dev
drwxr-xr-x    2 0        0             4096 Dec  9 02:34 etc
-rwxr-xr-x    1 0        0          1132888 Dec  9 02:14 ls
dr-xr-xr-x  186 0        0                0 Dec  9 02:34 proc
dr-xr-xr-x   13 0        0                0 Dec  9 02:34 sys
$ docker container run --rm -t ls:v2 sys
block       class       devices     fs          kernel      power
bus         dev         firmware    hypervisor  module
$
```