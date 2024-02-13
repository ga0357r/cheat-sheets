# Ubuntu

Ubuntu is a Linux distribution based on Debian and composed mostly of free and open-source software.Ubuntu is officially released in three editions: Desktop, Server, and Core for Internet of things devices and robots. Ubuntu is a popular operating system for cloud computing, with support for OpenStack.

## How to enable sudo without a password for a user

Open a Terminal window and type:

```
sudo visudo
```

In the bottom of the file, add the following line:

```
$USER ALL=(ALL) NOPASSWD: ALL
```

Where `$USER` is your username on your system. Save and close the sudoers file (if you haven't changed your default terminal editor (you'll know if you have), press Ctl + x to exit `nano` and it'll prompt you to save).

---
## Networking

In Ubuntu, networking can be managed using various tools and utilities, including the following:

1. **NetworkManager**: NetworkManager is a system service that manages network connections and devices. It provides a graphical user interface (GUI) for configuring network settings, as well as a command-line interface (CLI) for advanced configuration. NetworkManager is the default network management tool in Ubuntu.

2. **Netplan**: [Netplan](../netplan) is a command-line utility for configuring network interfaces in modern versions of Ubuntu. It uses YAML configuration files to describe network interfaces, IP addresses, routes, and other network-related parameters. [Netplan](../netplan) generates the corresponding configuration files for the underlying network configuration subsystem, such as systemd-networkd or NetworkManager.

3. **ifupdown**: ifupdown is a traditional command-line tool for managing network interfaces in Ubuntu. It uses configuration files located in the ﻿/etc/network/ directory to configure network interfaces, IP addresses, routes, and other network-related parameters.

To manage networking in **Ubuntu**, you can use one or more of these tools depending on your needs and preferences. For example, you can use the NetworkManager GUI to configure basic network settings and use [Netplan](../netplan) or ifupdown for advanced configuration. You can also use the command-line tools to automate network configuration tasks or to configure networking on headless servers.

## Install Docker on Ubuntu WSL2

1) Remove Docker
```
sudo apt remove docker docker-engine docker.io containerd runc
```
2) Update your local Linux software repository
```
sudo apt-get update
```
3) Download Docker dependencies.
```
sudo apt install — no-install-recommends apt-transport-https ca-certificates curl gnupg2
source /etc/os-release
```
4) Add Docker APT key
```
curl -fsSL https://download.docker.com/linux/${ID}/gpg | sudo apt-key add
```
5) Add docker apt repository
```
echo “deb [arch=amd64] https://download.docker.com/linux/${ID} ${VERSION_CODENAME} stable” | sudo tee /etc/apt/sources.list.d/docker.list
```
6) Install the latest Docker CE version
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
7) Start Docker service
```
sudo service docker start
```
8) Ensure docker group exists
```
sudo groupadd docker
```
9) Add your username to docker group
```
sudo usermod -aG docker $USER && newgrp docker
```
10) Validate Docker installation
```
docker run hello-world
```
11) Check docker version
```
docker version
```
12) Check docker service status
```
sudo service docker status
```
13) Check Docker container and image status
```
sudo docker info
sudo docker ps
sudo docker images
```

## Install MiniKube on Ubuntu WSL2

1) Install Minikube
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo apt install ./minikube_latest_amd64.deb -y
rm minikube_latest_amd64.deb
```
2) Start it
```
minikube start
```
3) Get the dashboard going
```
minikube dashboard
```
4) move process from foreground to background
```
// Press CTRL-Z to pause process
//type "bg" to move process to background
```
5) Install Kubectl
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl

kubectl get po -A
```
6) Install Helm
```
curl --output helm-linux-amd64.tar.gz https://get.helm.sh/helm-v3.4.2-linux-amd64.tar.gz
tar -zxvf helm-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
rm helm-linux-amd64.tar.gz
rm -rf linux-amd64

helm version
```

#To Open file for editing
```
sudo nano /etc/apt/sources.list.d/docker.list
```
edit in place and press enter or follow ui popups
