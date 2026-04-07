---
layout: post
title: "Kubernetes so hot right now"
date: 2020-01-31
tags: [Kubernetes, Certifications, CKA, Containers, Career]
---

![Kubernetes so hot right now meme](https://makeameme.org/media/created/kubernetes-so-hot-5e33c7.jpg)

I started my journey getting the lay of the land with the Kubernetes course provided through the [Cloud Native Computing Foundation (CNCF)](https://www.cncf.io/). The Introduction to Kubernetes course is free to audit. If you want to take the course, complete tests throughout, and receive a certificate, it's $99. The course number is LFS158x — Introduction to Kubernetes. I also signed up for LFS258 — Kubernetes Fundamentals. That one isn't free, but you can bundle it with an exam voucher for $499. The exam by itself is $299.

I've scheduled to take the CKA exam at the end of March. The exam is 3 hours long and conducted online. There are requirements to take it — a webcam and Chrome browser with a browser extension. If you book through CNCF, you get 1 free retake.

## Why Kubernetes?

You're probably wondering why I chose Kubernetes (k8s) as my first topic. Honestly, I didn't even know what Kubernetes was until about a year and a half ago. Everywhere I look now, people have either started with Kubernetes or are in the process of migrating to a fully functional container orchestration environment. There are Meetup groups, KubeCon, and cloud providers like AWS, Google (obviously), Azure, and even VMware all jumping on the bandwagon. [KubeCon saw 65% first-time attendees this year!](https://www.cncf.io/wp-content/uploads/2020/01/KubeCon_NA_19_Report.pdf)

## What is Kubernetes?

To answer this, we have to take a step back. Legacy or monolithic applications are big, slow, and bulky — they require a lot of resources. If you need to scale, you either scale vertically by throwing more hardware at the host, or horizontally by adding more hosts. Either way is slow, potentially causing downtime or bottlenecks. And even after solving the scaling issue, you still have the challenge of maintenance.

In today's world, you have microservices and containers. Microservices let you break your application into small, independent pieces. Containers let you run those microservices. I love the analogy from the course: *"Pebbles, as opposed to the 1000-ton boulder, are much easier to handle."* Kubernetes gives you the ability to manage, scale, and automate your "pebbles."

There are other tools out there that handle container orchestration, but Kubernetes started as a Google project and is now part of CNCF. It's open-source, has a massive feature set, and a huge contributor community — which is what makes it so attractive. There are [use cases documented across virtually every industry](https://kubernetes.io/case-studies/).

---

**Next:** Experimenting in the lab *(insert evil laugh)*

---

**Sources:**

- [https://kubernetes.io/](https://kubernetes.io/)
- [https://training.linuxfoundation.org/training/kubernetes-fundamentals/#overview](https://training.linuxfoundation.org/training/kubernetes-fundamentals/#overview)
- [https://www.cncf.io/wp-content/uploads/2020/01/KubeCon_NA_19_Report.pdf](https://www.cncf.io/wp-content/uploads/2020/01/KubeCon_NA_19_Report.pdf)
- [https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS158x+2T2019/course/](https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS158x+2T2019/course/)

*Disclaimer: I'm not purposely promoting any brands or products — I'm simply sharing my experience.*
