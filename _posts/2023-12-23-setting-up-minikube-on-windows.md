---
title: "Setting up Minikube on Windows"
date: 2023-12-23 14:36:00
categories: [kubernetes]
tags: [kubernetes, minikube, docker, containers, k9s]
---

## Minikube

[Minikube](https://minikube.sigs.k8s.io/docs/) is an application for running a local Kubernetes
cluster, which makes it perfect for developing locally and learning about Kubernetes without having
to spin up (and pay for) a K8s cluster in the cloud.

Minikube runs inside a virtual machine, so you'll need a container or VM manager installed for it
to run in. I went with [Docker Desktop on Windows](https://docs.docker.com/desktop/install/windows-install/) but
QEMU, Hyperkit, Hyper-V, KVM, Parallels, Podman, VirtualBox, or VMware Fusion/Workstation are also
supported.

## Install Docker

 - Download Docker Desktop for Windows from [here](https://docs.docker.com/desktop/install/windows-install/).
 - Run the installer *(NB. requires a Windows restart)*.

## Install minikube

 - Go to the [minikube start page](https://minikube.sigs.k8s.io/docs/start/).
 - Under "(1) Installation" select your system configuration.
 - Either download the installer from the linked text in "Download and run the installer for the latest release.", or
   run the PowerShell command shown on the page.
 - Add the minikube.exe binary to your `PATH`
    > #### Note on minikube path with standalone installer
    > The script for adding minikube.exe to your path assumes minikube is installed at `C:\minikube`. However, **if you download
    > and run the standalone installer, the default minikube install location is `C:\Program Files\Kubernetes\Minikube`**, so
    > you'll need to add this to your `PATH` instead.
 - Open a terminal with administrator access, and run
   ```bash
   > minikube start
   ```
   - This downloads the Kubernetes base image, creates a docker container, configures certs, network, and storage, and
     starts the minikube cluster.

## (Optional) Install K9s

[K9s](https://k9scli.io) is a "terminal based UI" management tool for Kubernetes. We can use it to inspect and manage
the minikube cluster we just created. This is an alternative to `kubectl` which is mentioned in the minikube docs.

### Installation

Install via the [Chocolatey](https://chocolatey.org) package manager for Windows:
```bash
> choco install k9s
```

To start K9s:
```bash
> k9s
```
This should start up and show your running `minikube` cluster.

To quit K9s (from inside the K9s console)
```bash
> :q
```