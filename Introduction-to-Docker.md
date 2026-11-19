# Introduction to Docker

**Coddy Terminal journey, Section 3** · 11 chapters · 40 lessons · 40 graded challenges · Certificate earned 2026-11-14 ([verify](https://coddy.tech/certifications/tj6FfM-terminal-fhs4B2))

[← Back to portfolio summary](README.md)

> **About the commands below:** Coddy's browser terminal grades each challenge live but does not keep a copy of what was typed. The commands here are reference solutions I wrote afterward for each challenge I passed. They are the commands that satisfy each graded check, with a short note on what each one does.

## What this course covers

Docker from the command line: pulling and tagging images, running and managing containers, inspecting and executing into them, writing Dockerfiles (`FROM`, `CMD`, `COPY`, `WORKDIR`), moving files in and out of containers, and creating volumes and networks. Containers are the foundation for Azure Container Instances, Azure Container Registry and AKS.

## Contents

1. [Introduction](#1-introduction)
2. [Images](#2-images)
3. [Running Containers](#3-running-containers)
4. [Managing Containers](#4-managing-containers)
5. [Inspecting Containers](#5-inspecting-containers)
6. [Building Images](#6-building-images)
7. [Files In Images](#7-files-in-images)
8. [Copying Files](#8-copying-files)
9. [Volumes & Networks](#9-volumes--networks)
10. [Greeting App Project](#10-greeting-app-project)
11. [Final Challenges](#11-final-challenges)
12. [Command cheat sheet](#command-cheat-sheet)

---

## 1. Introduction

### 1.1 What Is Docker · Beginner
**Task:** Confirm Docker is installed and check its version.
```bash
docker --version
```
Prints the installed Docker client version. It's the quickest check that the Docker CLI is on the `PATH`.

### 1.2 Images And Containers · Beginner
**Task:** Run the `hello-world` image and read its message.
```bash
docker run hello-world
```
`docker run` pulls the image if it isn't local, creates a container from it and starts it. The `hello-world` container prints a message explaining those steps, then exits.

---

## 2. Images

### 2.1 Pulling Images · Beginner
**Task:** Download the `nginx` image.
```bash
docker pull nginx
```
Downloads the image layers from Docker Hub without running anything. With no tag, Docker uses `:latest`.

### 2.2 Listing Images · Beginner
**Task:** List only the `python` images.
```bash
docker images python
```
Adding a repository name after `docker images` filters the list to that repository.

### 2.3 Tagging Images · Easy
**Task:** Give `ubuntu:24.04` a second name, `myubuntu:stable`, and confirm it exists.
```bash
docker tag ubuntu:24.04 myubuntu:stable
docker images myubuntu
```
A tag is just another name that points to the same image ID, so no data is copied.

### 2.4 Recap: Image Wrangler · Easy
**Task:** Pull `redis`, tag it as `cache:v1`, then list the `cache` images.
```bash
docker pull redis
docker tag redis cache:v1
docker images cache
```

---

## 3. Running Containers

### 3.1 Running A Command · Beginner
**Task:** Print `containers are fun` from inside an Alpine container.
```bash
docker run alpine echo "containers are fun"
```
Anything after the image name replaces the image's default command.

### 3.2 Real Execution · Easy
**Task:** In Alpine, use `sh -c` to print 1 through 3 with `seq`.
```bash
docker run alpine sh -c "seq 3"
```
`sh -c` runs the quoted string through a shell inside the container, which is needed whenever you want shell features such as pipes or `&&`.

### 3.3 Different Distros · Beginner
**Task:** Read `/etc/os-release` in an Ubuntu container.
```bash
docker run ubuntu cat /etc/os-release
```
Each image carries its own userland. Here the file identifies the container as Ubuntu, even though it shares the host's kernel.

### 3.4 Auto-Remove Containers · Easy
**Task:** Print `done` from Alpine and have the container delete itself afterward.
```bash
docker run --rm alpine echo done
```
`--rm` removes the container as soon as it exits, so stopped containers don't pile up.

### 3.5 Recap: Container Runner · Easy
**Task:** In a self-removing Debian container, print `red`, `green` and `blue` on separate lines, sorted.
```bash
docker run --rm debian sh -c "printf 'red\ngreen\nblue\n' | sort"
```
The single quotes inside the double-quoted `sh -c` string keep the argument intact. The output is `blue`, `green`, `red`.

---

## 4. Managing Containers

### 4.1 Naming Containers · Beginner
**Task:** Run an Alpine container named `worker` that prints `ready`.
```bash
docker run --name worker alpine echo ready
```
`--name` replaces Docker's random name, so the container is easy to refer to later.

### 4.2 Detached Containers · Easy
**Task:** Start nginx in the background as `site` and confirm it's running.
```bash
docker run -d --name site nginx
docker ps
```
`-d` (detached) returns your prompt while the container keeps running. `docker ps` lists running containers.

### 4.3 Listing All Containers · Easy
**Task:** Run a short-lived container named `quicktask`, then show it even though it has exited.
```bash
docker run --name quicktask alpine echo hi
docker ps -a
```
`docker ps` hides stopped containers. `-a` shows all of them, including ones with an `Exited` status.

### 4.4 Stopping And Removing · Easy
**Task:** Start Redis detached as `db`, stop it, then list all containers to see it `Exited`.
```bash
docker run -d --name db redis
docker stop db
docker ps -a
```

### 4.5 Recap: Container Lifecycle · Easy
**Task:** Start nginx as `proxy`, force-remove it while it's running, then confirm it's gone.
```bash
docker run -d --name proxy nginx
docker rm -f proxy
docker ps -a
```
`rm -f` stops and removes in one step. Plain `rm` refuses to remove a running container.

---

## 5. Inspecting Containers

### 5.1 Container Logs · Easy
**Task:** Run `printer`, which echoes `saved message`, then read its output back from the logs.
```bash
docker run --name printer alpine echo "saved message"
docker logs printer
```
Docker captures a container's stdout and stderr, so you can still read the logs after the container has exited.

### 5.2 Inspecting Details · Easy
**Task:** Start nginx as `app` and print only its status.
```bash
docker run -d --name app nginx
docker inspect --format "{{.State.Status}}" app
```
`docker inspect` returns the full JSON configuration. The Go-template `--format` pulls out a single field, here `running`.

### 5.3 Executing Commands · Easy
**Task:** Start nginx as `server` and run `echo` inside it.
```bash
docker run -d --name server nginx
docker exec server echo "inside the container"
```
`docker exec` runs a new process inside a container that is already running. It's the go-to tool for debugging.

### 5.4 Recap: Health Check · Medium
**Task:** Start Redis as `store`, prove it responds, and print its status.
```bash
docker run -d --name store redis
docker exec store echo "store is alive"
docker inspect --format "{{.State.Status}}" store
```

---

## 6. Building Images

### 6.1 What Is A Dockerfile · Beginner
**Task:** Make `FROM alpine` the only line of the Dockerfile, then print the file.
```dockerfile
FROM alpine
```
```bash
cat Dockerfile
```
`FROM` sets the base image. Every Dockerfile starts with it.

### 6.2 Building An Image · Easy
**Task:** Build the one-line Dockerfile into an image tagged `mybox`.
```dockerfile
FROM alpine
```
```bash
docker build -t mybox .
```
The `.` is the build context, the folder whose files Docker can use during the build. `-t` names the result.

### 6.3 The CMD Instruction · Medium
**Task:** Add a default command, build the image as `auto`, and run it without arguments.
```dockerfile
FROM alpine
CMD ["echo", "built and running"]
```
```bash
docker build -t auto .
docker run auto
```
`CMD` is what runs when `docker run` gets no command. The JSON (exec) form avoids an extra shell.

### 6.4 Recap: Image Builder · Medium
**Task:** Build `portfolio:v1` from Debian with a default echo and run it.
```dockerfile
FROM debian
CMD ["echo", "my first image"]
```
```bash
docker build -t portfolio:v1 .
docker run portfolio:v1
```

---

## 7. Files In Images

### 7.1 The COPY Instruction · Medium
**Task:** Bake `note.txt` into an image as `/note.txt`, build it as `noted`, and read the file.
```dockerfile
FROM alpine
COPY note.txt /note.txt
```
```bash
docker build -t noted .
docker run noted cat /note.txt
```
`COPY` adds files from the build context into the image filesystem at build time.

### 7.2 The WORKDIR Instruction · Medium
**Task:** Set `/project` as the working directory, copy `info.txt` using a relative path, build the image as `located`, and read the file.
```dockerfile
FROM alpine
WORKDIR /project
COPY info.txt info.txt
```
```bash
docker build -t located .
docker run located cat /project/info.txt
```
`WORKDIR` creates the directory if needed and makes later relative paths resolve inside it.

### 7.3 Recap: Package It Up · Medium
**Task:** Package `config.txt` at `/etc/config.txt`, build the image as `packaged`, and read it.
```dockerfile
FROM alpine
COPY config.txt /etc/config.txt
```
```bash
docker build -t packaged .
docker run packaged cat /etc/config.txt
```

---

## 8. Copying Files

### 8.1 Copy Into A Container · Medium
**Task:** Copy `data.txt` into a running nginx container named `box` and read it from inside.
```bash
docker run -d --name box nginx
docker cp data.txt box:/data.txt
docker exec box cat /data.txt
```
`docker cp` works on running containers, which is useful for hot-fixing a config without rebuilding the image.

### 8.2 Copy Out Of A Container · Medium
**Task:** Create `result.txt` inside a container named `src`, copy it to the host, and print it.
```bash
docker run -d --name src nginx
docker exec src sh -c "echo ready > result.txt"
docker cp src:/result.txt ./result.txt
cat result.txt
```
The redirect has to run inside `sh -c`. Without it, the host shell would perform the redirect, not the container.

---

## 9. Volumes & Networks

### 9.1 Volumes · Beginner
**Task:** Create a volume named `appdata` and list volumes.
```bash
docker volume create appdata
docker volume ls
```
A volume is Docker-managed storage that outlives any single container, for example a database's data directory.

### 9.2 Networks · Beginner
**Task:** Create a network named `backend` and list networks, including the built-in `bridge`.
```bash
docker network create backend
docker network ls
```
Containers on the same user-defined network can reach each other by container name.

### 9.3 Recap: Managing Resources · Easy
**Task:** Create the `dbdata` volume and the `appnet` network, then list both with a separator.
```bash
docker volume create dbdata
docker network create appnet
docker volume ls
echo ---
docker network ls
```

---

## 10. Greeting App Project

A small end-to-end project: write a Dockerfile, build it, personalize it with a shipped file, and release a versioned tag.

### 10.1 Project Overview · Beginner
**Task:** Check the base images that are available.
```bash
docker images alpine
```

### 10.2 Write The Dockerfile · Easy
**Task:** Write the app's Dockerfile with a greeting `CMD`, then print it.
```dockerfile
FROM alpine
CMD ["echo", "Welcome to the greeting app"]
```
```bash
cat Dockerfile
```

### 10.3 Build And Run · Medium
**Task:** Build the image as `greetapp` and run it.
```bash
docker build -t greetapp .
docker run greetapp
```

### 10.4 Personalize The Greeting · Medium
**Task:** Ship `name.txt` in the image and make the image print it by default.
```dockerfile
FROM alpine
COPY name.txt /name.txt
CMD ["cat", "/name.txt"]
```
```bash
docker build -t greetapp .
docker run greetapp
```
Output: `Hello, Sam!`

### 10.5 Ship A Tagged Version · Medium
**Task:** Release `greetapp:1.0` and confirm the tag exists.
```bash
docker build -t greetapp:1.0 .
docker images greetapp
```
Versioned tags (rather than `latest`) are how releases are pinned in registries such as Azure Container Registry.

---

## 11. Final Challenges

### 11.1 Distro Detective · Medium
**Task:** Print the `PRETTY_NAME` of Alpine and of Ubuntu, using `-w /etc` and self-removing containers.
```bash
docker run --rm -w /etc alpine sh -c "grep PRETTY_NAME os-release"
docker run --rm -w /etc ubuntu sh -c "grep PRETTY_NAME os-release"
```
`-w` sets the working directory at run time, so the relative filename resolves to `/etc/os-release`.

### 11.2 Full Build Pipeline · Medium
**Task:** Build `release:final`, which copies `version.txt` into `/app` and prints it by default.
```dockerfile
FROM alpine
WORKDIR /app
COPY version.txt /app/version.txt
CMD ["cat", "/app/version.txt"]
```
```bash
docker build -t release:final .
docker run release:final
```

### 11.3 Runtime File Drop · Medium
**Task:** Copy `report.txt` into a live nginx container named `host`, read it, and confirm the container is still running.
```bash
docker run -d --name host nginx
docker cp report.txt host:/report.txt
docker exec host cat /report.txt
docker inspect --format "{{.State.Status}}" host
```

---

## Command cheat sheet

| Goal | Command |
| --- | --- |
| Pull / list / tag images | `docker pull IMG` · `docker images [REPO]` · `docker tag SRC NEW:TAG` |
| Run (one-off, auto-clean) | `docker run --rm IMG CMD` |
| Run in background, named | `docker run -d --name NAME IMG` |
| List / stop / remove | `docker ps [-a]` · `docker stop NAME` · `docker rm [-f] NAME` |
| Logs / inspect / exec | `docker logs NAME` · `docker inspect --format "{{.State.Status}}" NAME` · `docker exec NAME CMD` |
| Build | `docker build -t NAME:TAG .` |
| Copy files | `docker cp FILE NAME:/path` · `docker cp NAME:/path ./FILE` |
| Volumes / networks | `docker volume create/ls` · `docker network create/ls` |
