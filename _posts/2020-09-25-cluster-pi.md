---
layout: post
title: "Cluster Pi"
date: 2020-09-25
tags: [Homelab, Raspberry Pi, Kubernetes, K3s, Docker, Rancher]
---

I am planning on expanding my Raspberry Pi cluster to add more nodes in the future. I was looking for a low-cost compute option — prior to this I was using AWS, but over the long run it can get quite expensive. I didn't really need a lot in terms of CPU and memory, just enough for testing purposes.

I set up my environment with 2 Raspberry Pi's running Ubuntu 20.04 LTS (arm64), K3s with Docker, and Rancher. K3s is a lightweight version of Kubernetes — quick and easy to set up, especially when you're working with minimal hardware. Docker is the container runtime we'll use for our Kubernetes environment. Rancher provides a container management platform for our Kubernetes cluster.

I chose Rancher for a couple of reasons: it seems easier to set up versus other similar interfaces, and there's a lot of documentation and a fairly active user community, which helped me figure out how to get a stable setup working. I felt compelled to share this because I spent quite a bit of time going through several iterations of flashing SD cards again, and again, and again… you get the idea.

## Hardware

Here's everything I needed to set up my cluster:

- [Raspberry Pi 4B (2)](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)
- [Cloudlet cluster case for Raspberry Pi](https://www.amazon.com/s?k=cloudlet+cluster+case+raspberry+pi)
- [TP-Link 8 port switch](https://www.amazon.com/s?k=tp-link+8+port+switch)
- [1ft Ethernet cables](https://www.amazon.com/s?k=1ft+ethernet+cables)

I connected the 8 port switch to my router so the nodes can access my local network and the internet.

## Prepare Your Pi

In my setup I'm using 2 RPi's. The first RPi is the **Primary Node** (Control Plane). The second RPi is the **Worker Node**. The objective is to have the Primary Node run all the Kubernetes processes, while the Worker Node runs all the application processes launched by Kubernetes. There are ways to configure taints and tolerations to control which node does what — I won't get into that here, but it's worth exploring as a follow-up topic.

I used the Raspberry Pi Imager to flash the SD cards with the OS. For instructions on how to flash your Raspberry Pi, check out: [https://www.raspberrypi.org/blog/raspberry-pi-imager-imaging-utility/](https://www.raspberrypi.org/blog/raspberry-pi-imager-imaging-utility/).

## Supporting Containers

Prepare your system to enable container features. This has to be performed on **all** nodes:

```bash
sudo sed -i '$ s/$/ cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1/' /boot/firmware/cmdline.txt
```

Reboot the system after making this change so it can take effect:

```bash
sudo reboot now
```

## Install Docker

Set up the Docker repository in order to perform installation on **all** nodes:

```bash
sudo apt-get update
```

```bash
sudo apt-get install \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  software-properties-common
```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```bash
sudo add-apt-repository \
  "deb [arch=arm64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"
```

Docker Engine install:

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Set permissions to run Docker commands:

```bash
sudo groupadd docker  # this may not be required
sudo usermod -aG docker $USER
```

This allows the user you're logged in as to run Docker commands without `sudo`. Log out and back in after running this.

Set Docker to run at startup, otherwise you'll have to start it manually every time you reboot:

```bash
sudo systemctl enable docker
```

Verify Docker commands run without requiring sudo:

```bash
docker ps
```

## Install K3s

Set environment variables on the **primary** node (this will be your control plane):

```bash
export K3S_KUBECONFIG_MODE="644"
export INSTALL_K3S_EXEC=" --no-deploy servicelb --no-deploy traefik"
```

Install K3s with Docker:

```bash
curl -sfL https://get.k3s.io | sh -s - --docker
```

> If you don't use the `--docker` flag, K3s will use containerd by default. We need Docker here because we want to launch the Rancher container later.

Check status of K3s:

```bash
sudo systemctl status k3s
```

Check if kubectl works and if the node is listed:

```bash
kubectl get nodes -o wide
```

In order to add the worker node later, grab the token from the primary node:

```bash
sudo cat /var/lib/rancher/k3s/server/node-token
```

Keep this token safe — you'll need it to join the additional nodes.

Log into the **worker** node and set environment variables to join it to the primary:

```bash
export K3S_KUBECONFIG_MODE="644"
export K3S_URL="https://[IP of primary node]:6443"
export K3S_TOKEN="[node-token from primary node]"
```

Install K3s on the worker node:

```bash
curl -sfL https://get.k3s.io | sh -s - --docker
```

Check the status of K3s:

```bash
sudo systemctl status k3s-agent
```

## Import K3s Cluster to Rancher

We need to import the K3s cluster so we can manage it through Rancher. To do this, click **Add Cluster** (located in the top right of the Clusters page).

On the next page, click **Other Cluster** to import the existing K3s cluster running on your Raspberry Pi's.

Name your cluster and click **Create**.

In the next screen, you'll see a `kubectl apply` command and a `curl` command. Since we're using a self-signed certificate in this example, use the `curl` command on the **primary** node of your Raspberry Pi.

You'll see the following objects created after running the curl statement:

```
clusterrole.rbac.authorization.k8s.io/proxy-clusterrole-kubeapiserver created
clusterrolebinding.rbac.authorization.k8s.io/proxy-role-binding-kubernetes-master created
namespace/cattle-system created
serviceaccount/cattle created
clusterrolebinding.rbac.authorization.k8s.io/cattle-admin-binding created
secret/cattle-credentials-d6e5b6d created
clusterrole.rbac.authorization.k8s.io/cattle-admin created
deployment.apps/cattle-cluster-agent created
daemonset.apps/cattle-node-agent created
```

After running the curl command, click **Done** on the Rancher Import Cluster page.

You'll now see the K3s cluster you created with your Raspberry Pi's. In my cluster I have 2 nodes running.

You can now access a dashboard that shows you the cluster resources. There's also a Cluster Explorer view that gives you detailed info about your cluster.

## Setup Rancher

Next, set up Rancher, which provides a UI to manage our K3s cluster. Rancher will run as a Docker container. Log back into the **primary** node of your Raspberry Pi cluster.

Run the following Docker command to start the container. This may take a few minutes because the Rancher image needs to be pulled:

```bash
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  --privileged \
  -v /opt/rancher:/var/lib/rancher \
  rancher/rancher:master-e9a98a32389e589921e5fef79011a9df356b02ea-linux-arm64
```

Access the Rancher UI by navigating to the IP of your Raspberry Pi. The command above starts the container on port 80. We'll finish setting up Rancher during our first time accessing the UI.

You'll be required to enter a new password for the Rancher UI and accept the terms of the agreement.

Next, you'll be asked to set the Rancher Server URL. This can be the name or IP of the host you're running Rancher on. The default will be the IP of the host.

After setting the password and URL, the local cluster will appear in the list of clusters. This is the cluster that Rancher itself is hosted on — the K3s cluster won't show up just yet.

## Thank You

A big thank you to everyone who helped me by pointing me in the right direction or sharing information that made this homelab possible. Without them, I'd have been banging my head against the wall a lot longer.

Special thanks to [TechnoTim](https://www.youtube.com/@TechnoTim), [Nato Riley](https://www.linkedin.com/in/nato-riley/), and [Grégoire Jeanmart](https://www.linkedin.com/in/gjeanmart/) — not only helpful, but genuinely approachable.
