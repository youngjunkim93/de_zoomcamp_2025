---
layout: post
title: "Data Engineering Zoomcamp - Week 1 - Docker"
permalink: /1_1_docker
---
As I embark on my journey into the data field, I’ve decided to kickstart the process by joining the Data Engineering Zoomcamp—a **free** five-week virtual bootcamp organized by [Datatalks.club](https://datatalks.club/). With data engineering playing a pivotal role in ensuring the success of data science and machine learning projects, this program is an exciting opportunity to dive into key concepts and tools while gaining hands-on experience with real-world data.

The first tool we are diving into is **Docker**.

## What is Docker?

According to Wikipedia,

> Docker is a set of platform-as-a-service (PaaS) products that use OS-level virtualization to deliver software in packages called containers.

In simpler terms, Docker creates **containers**—isolated environments that ensure what happens inside a container doesn’t affect the outside, and vice versa. This makes Docker a powerful tool for:

- Reproducibility
- Local experimentation
- Integration tests (CI/CD)
- Running pipelines on the cloud (e.g., AWS Batch, Kubernetes)
- Serverless computing (e.g., defining environments for AWS Lambda using Docker images)

Interestingly, I’ve used Docker extensively on my NAS to run programs independently without fully grasping its mechanism. Watching the lecture on Docker gave me a sense of pride, realizing I had been using it correctly all along! :)

## **Installation & Setup**

I used my MacBook to install Docker Desktop. If you’re on Windows or Linux, the steps might differ slightly.

To get started, I downloaded Docker Desktop from [https://www.docker.com/get-started/](https://www.docker.com/get-started/). It automatically set up Docker CLI during installation and initial setup. 

To confirm a successful installation, run the following command in the terminal:

```bash
docker run hello-world
```

If everything is set up correctly, you’ll see a message like this:

`"Hello from Docker! This message shows that your installation appears to be working correctly."`

## Running Containers from command-line

### Dissecting `hello-world`

What happens when you run the `hello-world `command?

1. Docker checks if the `hello-world` image is cached locally.
2. If it’s not found, Docker downloads the image from the default registry (Docker Hub).
3. Using the downloaded or cached image, Docker creates a new container.
4. The Docker daemon starts the container and executes the predefined instructions in the image.
5. After execution, the container exits.

### Running an Ubuntu container

Now, we will run Ubuntu container. To start an Ubuntu container:

```bash
docker run -it ubuntu bash
```

Here’s what this does:

- The `-it` flag launches an interactive terminal session.
- `ubuntu` specifies the container image to run.
- `bash` defines the command executed inside the container.

This command provides a shell session inside the container.

### Running a Python container

Next, we will run python container using the following command

```bash
docker run -it python:3.9
```

We can declare the version the image to install using colon, like `python:3.9`. When you run the command, you will see a Python interactive shell (REPL). But, what if you want to start from the bash shell instead of REPL? In this case, we can declare **entrypoint**. Entrypoint defines the main process or command for the container, and can be declared as below. 

```bash
docker run -it --entrypoint=bash python:3.9
```

Now, you will be see a bash shell welcoming you :) 

## Running Containers using Dockerfile

In real-world scenarios, containers are often more complex than simple commands like hello-world. Managing such setups directly via CLI can be tedious. This is where a **Dockerfile** comes in handy.

To replicate the behavior of the previous command (`docker run -it --entrypoint=bash python:3.9`), create the following Dockerfile:

```dockerfile
# Use the Python 3.9 base image
FROM python:3.9

# Set the entrypoint to bash
ENTRYPOINT ["bash"]
```

Save this as Dockerfile (no extension). We will build a docker image using the `Dockerfile` by running the command below in the directory where `Dockerfile` is located. The name of the docker image wil be `python_bash:v1`

```bash
docker build -t python_bash:v1 .
```

Once the image is built, run:

```bash
docker run -it python_bash:v1 
```

Tada! You’ve achieved the same functionality as before but now using a `Dockerfile` for cleaner, repeatable setup.
