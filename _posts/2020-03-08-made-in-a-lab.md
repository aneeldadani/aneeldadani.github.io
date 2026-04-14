---
layout: post
title: "Made in a Lab"
date: 2020-03-08
tags: [Kubernetes, Homelab, Minikube, Docker]
---

Before we dig into setting up our lab and getting our hands dirty with the tech, I'll talk about Kubernetes Architecture. Then I'll walk through how you can get started with your own lab environment. Feel free to deviate from my setup — I'd love to hear your feedback!

## Kubernetes Architecture

There are 3 components to consider when setting up a Kubernetes environment:

- **Master Node** — the control plane; runs the API server, scheduler, and controller
- **Worker Node** — runs your application workloads via Pods and Kubelet
- **Key-Value Store** — stores cluster state (etcd)

![Kubernetes Architecture](/assets/img/posts/2020-03-08-kubernetes-architecture.png)

*Image credit: [edX](https://www.edx.org/)*

There are various ways to deploy these components depending on your requirements. You could deploy a single master node with multiple worker nodes, or multiple master nodes with multiple worker nodes. For added redundancy, the key-value store can also be deployed across multiple nodes.

In my example, I'm using **Minikube**. Minikube is typically used for testing and demonstrating Kubernetes functionality — it shouldn't be used for production deployments. The master and worker components all run in a single node.

## Setting Up the Lab

Before you set up your lab, you'll need the following:

- Internet access
- A hypervisor: [VirtualBox](https://www.virtualbox.org/), [HyperKit](https://minikube.sigs.k8s.io/docs/reference/drivers/hyperkit/), or [VMware Fusion](https://www.vmware.com/products/fusion.html)
- [Minikube](https://minikube.sigs.k8s.io/)

I'm installing everything on my Mac using [Homebrew](https://brew.sh/). I decided to use the HyperKit hypervisor for this lab.

Install the hypervisor:

```bash
brew install hyperkit
```

Install Minikube:

```bash
brew install minikube
```

Start Minikube:

```bash
minikube start
```

A successful start should look something like this:

```
> Downloading VM boot image ...
> minikube-v1.8.0.iso: 173.56 MiB / 173.56 MiB [--] 100.00%
  Creating hyperkit VM (CPUs=2, Memory=4096MB, Disk=20000MB) ...
> Downloading preloaded images tarball for k8s v1.17.3 ...
> preloaded-images-k8s-v1-v1.17.3-docker-overlay2.tar.lz4: 499.26 MiB / 499
  Preparing Kubernetes v1.17.3 on Docker 19.03.6 ...
  Launching Kubernetes ...
  Enabling addons: default-storageclass, storage-provisioner
  Waiting for cluster to come online ...
  Done! kubectl is now configured to use "minikube"
```

Check the status of Minikube to confirm everything is running:

```bash
minikube status
```

Expected output:

```
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

## Interacting with Your Cluster

There are multiple ways to interact with the Master Node to manage your Kubernetes cluster.

**Option 1: CLI via kubectl**

```bash
kubectl config view
```

This will show your cluster configuration — certificate authority, server URL, context, and user info.

**Option 2: Kubernetes UI via `minikube dashboard` or `kubectl proxy`**

Running `minikube dashboard` assigns a random high port for the Kubernetes dashboard UI. Running `kubectl proxy` uses port 8001 for the running service. Note: `kubectl proxy` will lock your terminal while running. Use `CTRL+Z` and `bg` to push the process to the background so you can keep using your terminal.

```bash
kubectl proxy
# Starting to serve on 127.0.0.1:8001
```

Finally, you can interact with the Kubernetes cluster via the API directly. To see all available API endpoints:

```bash
curl http://localhost:8001
```

This will output all available API paths, including `/api`, `/apis`, `/healthz`, and many more.

---

At this point, we have a lab we can build upon. My next goal is to deploy an application and walk through that process.

**Sources:**

- Image credit: [edX LFS158x Kubernetes Fundamentals](https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS158x+2T2019/)
- HyperKit: [https://minikube.sigs.k8s.io/docs/reference/drivers/hyperkit/](https://minikube.sigs.k8s.io/docs/reference/drivers/hyperkit/)
- Homebrew: [https://brew.sh/](https://brew.sh/)
