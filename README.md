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

1. Introduction to Docker
2. Docker Installation
3. Docker Compose
4. Introduction to Kubernetes

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

