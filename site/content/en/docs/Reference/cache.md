---
title: "Caching"
linkTitle: "Caching"
weight: 6
date: 2019-08-01
description: >
  Cache Rules Everything Around Minikube
---

minikube has built-in support for caching downloaded resources into `$MINIKUBE_HOME/cache`. Here are the important file locations:

* `~/.minikube/cache` - Top-level folder
* `~/.minikube/cache/iso` - VM ISO image. Typically updated once per major minikube release.
* `~/.minikube/cache/images` - Docker images used by Kubernetes.
* `~/.minikube/cache/<version>` - Kubernetes binaries, such as `kubeadm` and `kubelet`

## Caching arbitrary Docker images

minikube supports caching arbitrary images using the `minikube cache` command. Cached images are stored in `$MINIKUBE_HOME/cache/images`, and loaded into the VM's container runtime on `minikube start`.

### Adding an image

```shell
minikube cache add ubuntu:16.04
```

### Listing images

```shell
minikube cache list
```

### Deleting an image

```shell
minikube cache delete <image name>
```

## Built-in Kubernetes image caching

`minikube start` caches all required Kubernetes images by default. This default may be changed by setting `--cache-images=false`. These images are not displayed by the `minikube cache` command.

## Sharing the minikube cache

For offline use on other hosts, one can copy the contents of `~/.minikube/cache`. As of the v1.0 release, this directory contains 685MB of data:

```text
cache/iso/minikube-v1.0.0.iso
cache/images/gcr.io/k8s-minikube/storage-provisioner_v1.8.1
cache/images/k8s.gcr.io/k8s-dns-sidecar-amd64_1.14.13
cache/images/k8s.gcr.io/k8s-dns-dnsmasq-nanny-amd64_1.14.13
cache/images/k8s.gcr.io/kubernetes-dashboard-amd64_v1.10.1
cache/images/k8s.gcr.io/kube-scheduler_v1.14.0
cache/images/k8s.gcr.io/coredns_1.3.1
cache/images/k8s.gcr.io/kube-controller-manager_v1.14.0
cache/images/k8s.gcr.io/kube-apiserver_v1.14.0
cache/images/k8s.gcr.io/pause_3.1
cache/images/k8s.gcr.io/etcd_3.3.10
cache/images/k8s.gcr.io/kube-addon-manager_v9.0
cache/images/k8s.gcr.io/k8s-dns-kube-dns-amd64_1.14.13
cache/images/k8s.gcr.io/kube-proxy_v1.14.0
cache/v1.14.0/kubeadm
cache/v1.14.0/kubelet
```

If any of these files exist, minikube will use copy them into the VM directly rather than pulling them from the internet.
