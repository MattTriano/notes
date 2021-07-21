# Docker Images
* **List all images:** `docker image ls -a`
* **Remove an image:** `docker rmi <image repo>`


# Docker Containers
* **List all containers:** `docker container ls -a`
* **Remove a container:** `docker rm <container id chars>`


## Inspecting a container
* `docker inspect <container name>`

to get a specific thing from the returned json description, 
* `docker inspect <container name> | grep <json key>`

### Inspecting a container's logs
* `docker logs <container name>`

### Creating a new image of a container
You can interactively make some changes to a container then produce a new image that encorporates those changes via the `commit` command. Let's say started up a container with an interactive telnet (`-it`) via
* `docker run -it --name olp-1 ubuntu:18.10 bash`
then we install something 
* `apt install git`
We can produce a new image that includes `git` via
* `docker commit olp-1 ubuntu-with-git:1.0.0`

This really demonstrates that containers consist of the base image and a writeable layer on top. This process caches the writeable layer to a read-only layer to be incorporated in the base image for new containers.

## Dockerfile Debugging Tips
If a dockerfile fails to build, grab the image id (eg `85b49a851fcc`) from right before the error and build a container from it.
* `docker run -it 85b49a851fcc`

# Dockerfile Compile-Time Instructions
FROM, ARG, RUN, ADD/COPY, USER

## FROM
FROM pulls an existing image in, which brings in all of the instructions in the parent Dockerfiles.

### FROM dos and don'ts

**Do**
* Pin your image to a specific tag (or digest)
* Choose your parent image wisely
* Vet that parent image! Check its USERs, PORTs, ENVs, VOLUMEs, LABELs, or anything else that you'll inherit.

**Don't**
* Never use the `latest` tag (the image changes)

## ARG
ARG is the only instruction that can precede FROM. ARG is like a parameter.

```docker
# sample Dockerfile
ARG VERSION=3.7
FROM alpine:${VERSION}

RUN cat /etc/alpine-release
```

You can overwrite these ARGs at buildtime via the `--build-arg` flag, eg
* `docker build -t demo --build-arg VERSION=3.8 -f Dockerfile .`

ARG values are not available after the build stage completes (unless they're captured as ENVs or LABELs).

### ARG Dos and Don'ts

**Do**
* Use ARG for configuring image construction at build time.
* Use ARGs to supply dynamic parameters to FROM, ENV, LABEL, and RUN.
* Default them appropriately.

**Don't**
* Set defaults on things that change every time

## RUN

RUN runs a command. 

### RUN Dos and Don'ts 

**Do**
* Be aware of the effects and potential issues of caching.
* Group common operations.
* Use multiline (\) to make it easier to audit, read, and debug Dockerfiles.

**Don't**
* Upgrade the operating system in a RUN command (rather, change the FROM)














