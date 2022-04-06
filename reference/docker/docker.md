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


## .dockerignore
The .dockerignore file tells the docker daemon what files to ignore. When you run `docker build`, the files in your working directory (aka **build context**) are copied into the docker daemon, which then builds the container. 

Note: files indicated in the .dockerignore file cannot be used by ADD or COPY. 


## ADD and COPY

ADD can take either a url to a file or a file on host. If you are ADDing a tar.gz file from the host filesystem, in the container that file will be un-tarballed, but if you are ADDing a tar.gz file from a URL, it will remain in tar.gz form.

COPY, on the other hand, will just copy a file over exactly (no automated uncompressing).


### ADD and COPY Dos and Don'ts 

**Dos**
* Use COPY with RUN if necessary.
* Be mindful of what's in the .dockerignore file. 

**Don'ts**
* Don't use ADD if you can avoid it. 


## USER

Ideally you want to set the USER to have the smallest possible set of priveliges needed to operate.

If building from a Ubuntu image, you could create a user via a command like

```docker
FROM ubuntu:18.10

RUN groupadd -r app \
  && useradd -r -g app appuser
USER appuser
``` 

### USER Dos and Don'ts

**Dos**
* Create a user (if you can) for your service.
* Default the container to a non-root user if possible.

**Don'ts**
* Don't switch USER too often (each switch creates a new layer).
* Don't use root if you can avoid it.


# Dockerfile Run-Time Instructions

## ENV

In contrast to ARGs (which only exist at build time), ENVs persist in the container as an environment variable. From the command line in a container, you can see all environment variables via the `env` command.

### ENV Dos and Don'ts

**Dos**
* Do use them for documentation and modifying runtime behavior.
	* They are baked into the final image.
* Do use `docker run <image-name> env` or `docker inspect`.
* Do set appropriate defaults.
* Do be cognizant of inherited ENV variables.

**Don'ts**
* Don't put secrets or sensitive information in ENV variables.
* Don't override ENVs from the parent image unless absolutely necessary.


## ENTRYPOINT and CMD

ENTRYPOINT 

CMD at the end of a Dockerfile can be overwritten at runtime by passing a parameter to the container in the `run` command.

There are two different syntaxes

**Shell form**
CMD java -jar /var/docker-olp-0.0.1-SNAPSHOT.jar

**Exec form**
CMD ["java", "-jar", "/var/docker-olp-0.0.1-SNAPSHOT.jar"]

But these are kind of clunky, and there's a better way.

### entrypoint.sh
Take an `entrypoint.sh` file like the one below

```bash
#!/bin/sh
set -e

# entrypoint.sh
if [ "$1" = 'default' ]; then
	# do default thing here
	echo "Running default"
	exec java -jar /var/docker-olp-0.0.1-SNAPSHOT.jar
else
	echo "Running user supplied arg"
	# if the user supplied, say, /bin/bash
	exec "$@"
fi

```
note: `set -e` sets the error flag

With this `entrypoint.sh` script, we can amend a Dockerfile so that it ends with 

```bash
...
COPY entrypoint.sh entrypoint.sh
ENTRYPOINT ["./entrypoint.sh"]
CMD ["default"]
```

which will behave like the CMD lines above, but be much less clunky. If the user doesn't provide any args at runtime, then the `CMD ["default"]` line won't be overwritten, thus "default" will be passed to the `entrypoint.sh` script and the `exec java ...` line will run, but if the user provides an arg, the CMD line will be ignored and the other block in the `entrypoint.sh` script will be entered. This allows us to supply much more complex logic.

### ENTRYPOINT and CMD Dos and Don'ts

**Dos**
* Do use the **exec** form (won't cause shell expansion!).
* Do use ENTRYPOINT and CMD together.
* Do use an `entrypoint.sh` script.

**Don'ts**
* Don't use the **shell** form if possible (it wraps the command in a shell, which will make the shell the root process rather than the actual process you explicitly indicate).

## HEALTHCHECK









# Volumes

## Listing volumes
* `docker volume ls`