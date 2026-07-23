
# DaemonSets
---

## What's DaemonSets?
---

- DaemonSets ensure that ****exactly one copy of a pod runs on every node in your Kubernetes cluster**.
	- When you add a new node, the DaemonSet automatically deploys the pod on the new node.
	- when a node is removed, the corresponding pod is also removed.
- It uses `nodeAffinity` and `default scheduler` to schedule the pod in background

## When use DaemonSets?
---

DaemonSets will be used to build up essential system / monitor tool which each node needs. Some examples might be:
- **Monitoring & Logging tools**
- **Networking suite**
- **Essential Kubernetes system** (E.g. kube-proxy)

## How to use DaemonSets?
---

An Example of DaemonSets template. Similar to `Deployment` and `ReplicaSet`: 

```
apiVersion: apps/v1
kind: DaemonSet 
metadata: 
	name: monitoring-daemon 
spec: 
	selector: 
		matchLabels: 
			app: monitoring-agent 
	template: 
		metadata: 
			labels: 
				app: monitoring-agent 
		spec: 
			containers: 
			- name: monitoring-agent 
			  image: monitoring-agent
```

After created, we can use the command in [[Scheduling CLI List#DaemonSets]] to check the status of daemonsets.