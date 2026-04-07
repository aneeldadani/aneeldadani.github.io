---
layout: post
title: "kubectl describe cka"
date: 2020-07-23
tags: [Kubernetes, Certifications, CKA, Career, Homelab]
---

I know it's been a while since I've written a new post. I've spent quite a bit of time studying for exams and transitioned to a new role within my company. I have a couple of upcoming topics I wanted to write about, but first I wanted to wrap up my discussion on Kubernetes.

Anyone thinking about pursuing the Certified Kubernetes Administrator (CKA) certification should look up courses that provide in-depth coverage of a Kubernetes deployment. It's also useful to get into the habit of setting up a cluster in a practice lab that mimics the exam itself. I gained quite a bit of knowledge and understanding of K8s by subscribing to the [KodeKloud](https://kodekloud.com/) course.

## Tips

The tips I found most useful were around exam-taking fundamentals. For example:

- The exam has **6 different clusters**, each with a different configuration context. At the beginning of each question, you have to set your configuration context for the corresponding cluster.
- The exam uses **Kubernetes 1.17**, so practicing with that version matters. I was initially practicing with an older version and realized a lot of commands were deprecated in 1.17.
- You have to **allow third-party cookies** — otherwise, the terminal for the exam will not load.

## Resources Available

You're allowed to use the following resources during the exam:

- [https://kubernetes.io/docs/](https://kubernetes.io/docs/)
- [https://github.com/kubernetes/](https://github.com/kubernetes/)
- [https://kubernetes.io/blog/](https://kubernetes.io/blog/)

I recommend using the [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) as a reference guide throughout the exam.

## Shortcuts

Some questions can take a while, which means shortcuts are going to be extremely helpful. I found that using imperative commands versus writing out YAML files sped things up considerably, especially for simpler questions.

Running an imperative command like this:

```bash
kubectl run nginx --generator=run-pod/v1 --image=nginx -n default
```

Is equivalent to writing this YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
  namespace: default
spec:
  containers:
  - image: nginx
    imagePullPolicy: Always
    name: nginx
```

The beginning of the kubectl Cheat Sheet shows you how to set up autocomplete and an alias for the kubectl command. Add this to your `.bashrc` file:

```bash
alias k=kubectl
complete -F __start_kubectl k
```

Review the kubectl Cheat Sheet and know your way around the docs. If a question is taking too long or you know it will take some time, skip it and come back to it — you can flag questions for review after you've answered the others.

## Practice, Practice, Practice

I mentioned using Minikube as a great way to get started. Since it's a single-node all-in-one deployment, it may not be enough if you want a better understanding of multi-node deployments. There are some caveats to multi-node setups that are helpful to understand before the exam. Two references I'd recommend:

- [Kelsey Hightower's Kubernetes the Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way)
- [Creating a single control-plane cluster with kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)

I'd recommend setting up your cluster the hard way first, then understanding how to do it with kubeadm.

## Onward and Upward!

In preparation for my next topic, I'm going to be setting up a multi-node Raspberry Pi cluster. Stay tuned!
