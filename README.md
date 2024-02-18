# Docker and Kubernetes

> This is a tutorial course covering Docker and Kubernetes.

Tools used:

- JDK 11
- Maven
- JUnit 5, Mockito
- IntelliJ IDE
- WSL2
- Docker Engine

## Table of contents

1. [Introduction to Docker](https://github.com/backstreetbrogrammer/50_DockerAndKubernetes?tab=readme-ov-file#chapter-01-introduction-to-docker)
2. [Docker Installation](https://github.com/backstreetbrogrammer/50_DockerAndKubernetes?tab=readme-ov-file#chapter-02-docker-installation)
3. [Docker Deep Dive](https://github.com/backstreetbrogrammer/50_DockerAndKubernetes?tab=readme-ov-file#chapter-03-docker-deep-dive)
    - [Docker CLI](https://github.com/backstreetbrogrammer/50_DockerAndKubernetes?tab=readme-ov-file#docker-cli)
4. Docker Compose
5. Introduction to Kubernetes

---

## Chapter 01. Introduction to Docker

**_Why use Docker?_**

When we install **software** on our machine, we typically go through the following procedure and **may** get errors:

![InstallingSoftware](InstallingSoftware.PNG)

In the real world, it happens a lot that software works on one computer, but it does not work on others due to different
environments.

We can extend this concept to our production **applications**.

Our application can contain a lot of **dependencies** which may be not available or installed on the production server.

Also, it may be incompatible with the **versions** required for the application versus what version is installed on the
server.

Docker tries to fix it by using **containers** which contain all the dependencies with correct versions needed for the
software or application to run.

It runs as a **sandbox** without any dependencies needed from the underlying server.

Every developer on a team will have the exact same development instance.

Each testing instance is exactly the same as the development instance.

Our production instance is exactly the same as the testing instance.

Also, developers around the world can share their **Docker images** on a platform called
[docker hub](https://hub.docker.com/).

**_What is Docker?_**

> Docker is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in
> packages called containers.

![DockerEcosystem](DockerEcosystem.PNG)

Docker is a platform or ecosystem around creating and running **containers**.

![ImageContainer](ImageContainer.PNG)

Docker builds **images** and runs **containers** by using the **docker engine** on the **host machine**.

Docker **containers** consist of all the dependencies and software needed to run an application in different
environments.

---

## Chapter 02. Docker Installation

**Install Docker:**

- [Installing Docker on Windows](https://docs.docker.com/desktop/install/windows-install/)
- [Installing Docker on MacOS](https://docs.docker.com/desktop/install/mac-install/)
- [Installing Docker on Linux](https://docs.docker.com/desktop/install/linux-install/)

**Install WSL2 on Windows:**

- Open Powershell in administrative mode
- Type this command: `wsl --install`
- Restart the system
- After restart, `Ubuntu` app will be installed
- Set up Linux username and password
- Disable IPv6 on WSL2

```
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
```

**Install Java JDK version 11 in WSL2**

- Run these three commands one by one:

```
wget -O- https://apt.corretto.aws/corretto.key | sudo apt-key add - 
sudo add-apt-repository 'deb https://apt.corretto.aws stable main'
sudo apt-get update; sudo apt-get install -y java-11-amazon-corretto-jdk
```

- Verify by running command: `java -version`

**_Install Docker in Ubuntu on WSL2_**

- Set up Docker's apt repository

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

```
# Add the repository to Apt sources:
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

- Install the Docker packages

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- Configuring Docker on WSL2

    - Using Docker Without Invoking Root

```
sudo groupadd docker
sudo usermod -aG docker $USER

# restart Ubunutu

# verify installation
docker run hello-world
```

- Starting the Docker Daemon

```
# add following in "~/.profile"

if [ -n "`service docker status | grep Stopped`" ]; then
    sudo /usr/sbin/service docker start
fi
```

- Fix asking root password at startup by running below commands:

```
# replace username with actual username of Ubuntu:

sudo visudo -f /etc/sudoers.d/passwordless_docker_start

username        ALL = (root) NOPASSWD: /usr/sbin/service docker start

# save and exit the file:
^S and then ^X
```

- Install **docker-compose**

```
sudo apt  install docker-compose
```

**_Verify installation_**

Launch Ubuntu app in WSL2 and lets run some docker CLI commands.

- Command: `docker version`

```
$ docker version
Client: Docker Engine - Community
 Version:           25.0.1
 API version:       1.44
 Go version:        go1.21.6
 Git commit:        29cf629
 Built:             Tue Jan 23 23:09:23 2024
 OS/Arch:           linux/amd64
 Context:           default

Server: Docker Engine - Community
 Engine:
  Version:          25.0.1
  API version:      1.44 (minimum version 1.24)
  Go version:       go1.21.6
  Git commit:       71fa3ab
  Built:            Tue Jan 23 23:09:23 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.6.27
  GitCommit:        a1496014c916f9e62104b33d1bb5bd03b0858e59
 runc:
  Version:          1.1.11
  GitCommit:        v1.1.11-0-g4bccb38
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

The output shows details for both the Docker Client and Docker Server.

- Command: `docker run hello-world`

```
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

---

## Chapter 03. Docker Deep Dive

![DockerCLI](DockerCLI.PNG)

Let's create a basic python application which just prints something in the console.

- Create a folder on computer. It must contain the following two files:

A `main.py` file (python file that will contain the code to be executed).
A `Dockerfile` file (Docker file that will contain the necessary instructions to create the environment).

`main.py` may have the following content:

```
#!/usr/bin/env python3

print("Hello Guidemy Students!")
```

- Edit the `Dockerfile` file

Our goal here is to launch Python code.

To do this, our Docker must contain all the dependencies necessary to launch Python. A linux (Ubuntu) with Python
installed on it should be enough.

The first step to take when we create a Docker file is to access the [DockerHub website](https://hub.docker.com/).

This site contains many pre-designed images to save our time (for example, all images for linux or code languages).

In our case, we will type **"Python"** in the search bar. The first result is the official image created to execute
Python.

```
# A dockerfile must always start by importing the base image.
# We use the keyword 'FROM' to do that.
# In our example, we want import the python image.
# So we write 'python' for the image name and 'latest' for the version.
FROM python:latest

# In order to launch our python code, we must import it into our image.
# We use the keyword 'COPY' to do that.
# The first parameter 'main.py' is the name of the file on the host.
# The second parameter '/' is the path where to put the file on the image.
# Here we put the file at the image root folder.
COPY main.py /

# We need to define the command to launch when we are going to run the image.
# We use the keyword 'CMD' to do that.
# The following command will execute "python ./main.py".
CMD [ "python", "./main.py" ]
```

- Create the Docker image

Once our code is ready and the `Dockerfile` is written, all we have to do is create our docker **image** to contain our
application.

```
docker build -t python-test .
```

The `-t` option allows us to define the name of our docker image.

- Run the Docker image

```
docker run python-test
```

Our docker container should be running now and will print the message: `Hello Guidemy Students!`

**_Linux Namespace_**

![Namespace](Namespace.PNG)

**Namespaces** are a feature of the Linux `kernel` that partition kernel resources such that one set of processes sees
one set of resources while another set of processes sees a different set of resources.

The feature works by having the same namespace for a set of resources and processes, but those namespaces refer to
distinct resources. Resources may exist in multiple spaces.

Examples of such resources are process IDs, host-names, user IDs, file names, some names associated with network access,
and Inter-process communication.

Namespaces are a fundamental aspect of **containers** in Linux.

**_Linux Control Groups_**

![cgroups](cgroups.PNG)

Linux Control Groups or `cgroups` is a Linux `kernel` feature that limits, accounts for, and isolates the resource
usage (CPU, memory, disk I/O, etc.) for a collection of processes.

**_Docker server_**

![LinuxKernel](LinuxKernel.PNG)

When Docker is installed, it creates a Linux virtual machine on top of our local machine OS.

This Linux Virtual Machine or Docker Host or Docker Server takes care of Docker containers run using **namespace** and
**cgroups**.

### Docker CLI

- Creating and running a container from an image:

```
$ docker run <image name>

docker = Reference the docker client
run = try to create and run a container
<image name> = name of image to use for this container 
```

For example, when we run the command: `docker run hello-world`, container is created and run:

![DockerRun](DockerRun.PNG)

- Running a container with an overriding command

```
$ docker run <image name> <command>

<command> = default command override 
```

For example, we can run the following command:

```
$ docker run busybox echo hello Guidemy Students
Unable to find image 'busybox:latest' locally
latest: Pulling from library/busybox
9ad63333ebc9: Pull complete
Digest: sha256:6d9ac9237a84afe1516540f40a0fafdc86859b2141954b4d643af7066d598b74
Status: Downloaded newer image for busybox:latest
hello Guidemy Students
```

Here, we override the default command of the image `busybox` by another command to echo.

Similarly, we can use any other Linux command to override the default command.

```
$ docker run busybox ls
bin
dev
etc
home
lib
lib64
proc
root
sys
tmp
usr
var
```

- List all images:

`$ docker image ls`

- Delete a specific image:

`$ docker image rm [image name]`

- Delete all existing images:

`$ docker image rm $(docker images -a -q)`

- List all existing containers (running and not running):

`$ docker ps -a`

- List only the container ids (running and not running):

`$ docker ps -a -q`

- Stop a specific container:

`$ docker stop [container name]`

- Stop all running containers:

`$ docker stop $(docker ps -a -q)`

- Delete a specific container (only if stopped):

`$ docker rm [container name]`

- Delete all containers (only if stopped):

`$ docker rm $(docker ps -a -q)`

- Display logs of a container:

`$ docker logs [container name]`

**_Lifecycle of a container_**

Docker `run` command is actually a combination of `create` and `start` commands.

```
docker run = docker create + docker start
```

- **Create a container**

```
$ docker create <image name>

create = try to create a container
<image name> = name of image to use for this container
```

For example,

```
$ docker create hello-world
1eb2c149f968aed81c800854d811ef15003f84fedda588c50fc1d57e8a193b29
```

- **Start a container**

```
$ docker start <container id>

start = try to start a container
<container id> = id of the container to start
```

For example,

```
$ docker start -a 1eb2c149f968aed81c800854d811ef15003f84fedda588c50fc1d57e8a193b29
```

Option `-a` is given to show or attach the output to the console.

- **Restarting a stopped container**

We can get the container id of a stopped container (STATUS as **Exiting**) using `docker ps -a`.

Then, we can use the start command to run the container again.

For example,

```
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                      PORTS     NAMES
1eb2c149f968   hello-world   "/hello"                 11 minutes ago   Exited (0) 10 minutes ago             funny_gates
...
...
c6d16c261df8   busybox       "echo hello Guidemy …"   38 minutes ago   Exited (0) 38 minutes ago             inspiring_poincare
```

```
$ docker start -a c6d16c261df8
hello Guidemy Students
```

One thing important to understand here is that both **images** and **containers** are **_immutable_** - we can override
the default command for an existing container using the `start` command. 

