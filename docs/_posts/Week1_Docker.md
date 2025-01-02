
---
layout: page
title: "Data Engineering Zoomcamp - Week 1 - Docker"
permalink: /week1_docker
---


As I prepare for transition into data field, the first thing I decided to do is to participate in Data Engineering zoomcamp. It is a five-week virtual **free** bootcamp organized by [Datatalks.club](https://datatalks.club/). Since successful data science and machine learning projects rely on the availability of high-quality datasets, data engineering is getting more and more important. I am excited to learn the important concepts and tools of data engineering and to gain hands-on experience using them on real data. 

The first tool we will learn about is **Docker**.

## What is Docker?

According to Wikipedia, Docker is

> A set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers.

Docker basically makes **containers** that run independently, so whatever is done outside the container won't affect the container and whatever is done inside the container won't affect the outside. 

For this reason, Docker is used for ...

- Reproducibility
- Local experiments
- Integration tests (CI/CD)
- Running pipelines on the cloud (AWS batch, Kubernetes)
- Serverless computing ... services such as AWS Lambda allows us to define environments as docker images

I have been using Docker a lot on my NAS without knowing so much about it. I used Docker mostly to run some programs independently of my NAS system to prevent messing up the system while setting up the programs. After watching the lecture on Docker, I was impressed at myself that I was using Docker correctly even without fully understanding what it was! :) 

## Installation & Setup

I ran all the steps below on my MacBook, so it may not work 100% for Windows or Linux environment.

First, installed Docker Desktop from [https://www.docker.com/get-started/](https://www.docker.com/get-started/). It automatically set up Docker CLI during installation and initial setup. 

To test successful installation, we can run the following command on Terminal.

```bash
docker run hello-world
```

If the installation was successful, you will see output like `"Hello from Docker! This message shows that your installation appears to be working correctly."

## Running Containers from command-line

### Dissecting `hello-world`

You may wonder what happens behind the scenes of `hello-world`. This is what happens

1. Docker will check if cached `hello-world` image already exists on your machine.
2. If it doesn't exist on the machine, Docker will download the `hello-world` image from the container registry (in this case, the default Docker Hub)
3. Using the downloaded or local `hello-world` image, Docker creates a new container.
4. Docker daemon starts the container and whatever defined in the image is executed. 
5. After all the processes are executed, the container exits

### Running something (slightly) more advanced

Now, we will run Ubuntu container. Run the following command

```bash
docker run -it ubuntu bash
```

the `-it` option allows to start an interactive terminal session inside a running container. Here, `ubuntu` is the container image to be downloaded and run. `bash` is the command to be executed inside the container. Therefore, this command will you give an interactive shell session within the container. 

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

In reality, our Docker container will be much more complicated than running `hello-world` or invoking a bash shell. In this case, running container like this using CLI will be huge pain! To make it easier, we can use **Dockerfile** to set up the container. 

Dockerfile to replicate the functionality of the above command `docker run -it --entrypoint=bash python:3.9` is 

```dockerfile
# Use the Python 3.9 base image
FROM python:3.9

# Set the entrypoint to bash
ENTRYPOINT ["bash"]
```

We save this as `Dockerfile` (Without any extension). We will build a docker image using the `Dockerfile` by running the command below in the directory where `Dockerfile` is located. The name of the docker image wil be `python_bash:v1`

```bash
docker build -t python_bash:v1 .
```

Once the image is built successfully, we will run the Docker image. 

```bash
docker run -it python_bash:v1 
```

Tada! Now we did the same task using `Dockerfile`! 
