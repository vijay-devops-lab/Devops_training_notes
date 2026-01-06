# Kubernetes Installation Using kubeadm (Single-Node Setup)

## Introduction
This document explains how to install and set up a Kubernetes cluster using **kubeadm** on **Ubuntu 22.04**.  
The cluster runs as a **single-node cluster**, where the control plane and worker components coexist on the same instance.

---

## Architecture Overview

```
EC2 / VM (Single Node)
├── Control Plane
│   ├── kube-apiserver
│   ├── kube-scheduler
│   ├── kube-controller-manager
│   └── etcd
│
├── Worker Components
│   ├── kubelet
│   ├── kube-proxy
│   └── containerd
│
└── CNI Plugin
    └── Calico
```

---

## Prerequisites

- Ubuntu 22.04
- 2 vCPU (minimum)
- 4 GB RAM (recommended)
- 20 GB Disk
- Internet access
- sudo privileges

---

## Disable Swap

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

---

## Install Required Packages

```bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl
```

---

## Add Kubernetes Repository

```bash
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

---

## Install kubeadm, kubelet, kubectl

```bash
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

---

## Verify Installation

```bash
kubeadm version
kubelet --version
kubectl version --client
```

---

## Initialize Kubernetes Cluster

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

---

## Configure kubectl Access

```bash
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Verify:
```bash
kubectl get nodes
```

---

## Install CNI (Calico)

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

Check:
```bash
kubectl get pods -n kube-system
```

---

## Enable Scheduling on Control Plane

```bash
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
```

---

## Test Deployment

```bash
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=NodePort --port=80
kubectl get svc
```

Access:
```
http://<NODE_PUBLIC_IP>:<NODEPORT>
```

---

## NodePort Architecture

```
User
 → Node IP : NodePort
 → kube-proxy
 → Service (ClusterIP)
 → Pod IP
 → Container
```

---

## Kubernetes Concepts Used

- kubeadm
- kubelet
- kubectl
- Pod
- Deployment
- Service (NodePort)
- CNI (Calico)
- Taints & Scheduling

---

## Conclusion

This setup provides a production-like Kubernetes environment using kubeadm on a single node, suitable for learning, demos, and application deployments.
