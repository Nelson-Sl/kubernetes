
# What's Kubernetes?
---

A container orchestration technology that manages and deploys thousands of containers in a  cluster through orchestrate the **connectivity** and **auto-scaling**.
# Hierarchy of  Kubernetes
---
## The hierarchy of Kubernetes unit
---

```mermaid
flowchart TD;
    Cluster-- have 1 or more ---Node;
    Node-- have 1 or more ---Pod;
```
## The architecture of Cluster
---
```mermaid
flowchart LR;
Cluster-->MasterNode["Master Node: Watch over the nodes & responsible for the actual orchestration of worker nodes"];
Cluster-->WorkerNode["Worker Node: The other nodes installed in cluster as standalone containers"]
MasterNode-->ApiServer["API Server: The frontend of K8s to interact with CLI and management services"]
MasterNode-->etcd["etcd: A distributed key store to store all metadata managed by K8s cluster (E.g. Nodes, Pods, Config) and make sure no conflicts between notes"]
MasterNode-->scheduler["Scheduler: distribute work/newly-created container across multiple nodes"]
MasterNode-->controller["Controller: Responsible for nodes orchestration & auto-scaling"]
WorkerNode-->kubelet["Kubelet: Runs in each node for communicating master node & deliver health status"]
WorkerNode-->runtime["Container Rumtime: Underlying software to run container"]
runtime-->Rocket
runtime-->Docker["Docker Through ContainerD"]
```

# K8s CLI Tools
---

* **Kubectl**
* **ctr**: Use for **ContainerD** in debugging, not user friendly with limited features
* **NerdCtl**: Use for **ContainerD** runtime in actual practice, docker-like command line tool
	* Support **Docker Compose** & Other new features like encrypted images
* **CriCtl**: Use for other container compatible with **Command Runtime Interface (CRI)** for debugging purpose (E.g. Rocket)
* **Kubeadm**: a tool built to provide `kubeadm init` and `kubeadm join` as best-practice "fast paths" for creating Kubernetes clusters.