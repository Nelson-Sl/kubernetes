# Manual Scheduling
---

* Kubernetes scheduler will automatically schedule different pods by **adding nodeName property and connect with node with a binding object**

*  nodeName in Pod assignment
```
apiVersion: v1
kind: Pod
metadata:
 name: nginx
 labels:
  name: nginx
spec:
 containers:
 - name: nginx
   image: nginx
   ports:
   - containerPort: 8080
 nodeName: node02
```

* Binding project to connect node 
```
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: node02
```

# Schedule through labels and selectors
---
## Labels & matchLabel with selectors
---

Label can be used to group different kinds of pods in a node so that specific kinds of pod will be scheduled in a group of the same labels.

Here's the Example:
* Adding label in a pod
```
 apiVersion: v1
 kind: Pod
 metadata:
  name: simple-webapp
  labels:
    app: App1
    function: Front-end
 spec:
  containers:
  - name: simple-webapp
    image: simple-webapp
    ports:
    - containerPort: 8080
```
* When creating replicaset / deployment, matching existing labels when configuring template
```
 apiVersion: apps/v1
 kind: ReplicaSet
 metadata:
   name: simple-webapp
   labels:
     app: App1
     function: Front-end
 spec:
  replicas: 3
  selector:
    matchLabels:
     app: App1
  template:
    metadata:
      labels:
        app: App1
        function: Front-end
    spec:
      containers:
      - name: simple-webapp
        image: simple-webapp   
```

## Annotations
---

When label is used to group different objects, annotation can be used to save other necessary information, in a key-value format manner.

Example：
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: simple-webapp
  labels:
    app: App1
    function: Front-end
  annotations:
     buildversion: 1.34
spec:
 replicas: 3
 selector:
   matchLabels:
    app: App1
template:
  metadata:
    labels:
      app: App1
      function: Front-end
  spec:
    containers:
    - name: simple-webapp
      image: simple-webapp   
```

# Schedule Pods: Restricting Pods to specific node
---

## Taints & Tolerations
---

It is used to schedule what pods can be scheduled on a node. **Only pods that can tolerate a taint could be used to schedule on that node** (But its not guaranteed, it could also be scheduled to node that don't have any taints as well).

Examples:
* Using the following examples to add taint to one node, the taint effect could be one of the following attributes:
  * **NoSchedule**: No new Pods will be scheduled on the tainted node unless they have a matching toleration.
  * **PreferNoSchedule**: The control plane will **_try_ to avoid placing a Pod that does not tolerate the taint on the node**, but it is not guaranteed.
  * **NoExecute**: Affect the existing pods. E.g. Pods that set up *tolerantSeconds* will only be tolerant in specified times after set up
```
$ kubectl taint nodes <node-name> key=value:taint-effect
```
* Add tolerant rules in pod
```
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: nginx-container
   image: nginx
 tolerations:
 - key: "app"
   operator: "Equal"
   value: "blue"
   effect: "NoSchedule"
```

## Node Selectors
---

Adding node selectors and set up match labels when set up pods can set up what node could schedule pods. But the cons is **it can't satisfy multiple conditions as needed** and for this case it should apply [[#^Node Affinity]].

Example:
* Adding label for nodes
```
$ kubectl label nodes node-1 size=Large
```
* Match Label using **nodeSelectors** when defining pod
```
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: data-processor
   image: data-processor
 nodeSelector:
  size: Large
```

## Node Affinity
---

Another way to ensure pod to **host on particular nodes**, with support on multiple conditions and comparisons (E.g. advanced expressions). The affinity type includes following:

* requiredDuringSchedulingIgnoredDuringExecution (Available)
- preferredDuringSchedulingIgnoredDuringExecution (Available)
- requiredDuringSchedulingRequiredDuringExecution (Planned)
- preferredDuringSchedulingRequiredDuringExecution (Planned)

![[Pasted image 20260712174529.png]]

Example:
```
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: data-processor
   image: data-processor
 affinity:
   nodeAffinity:
     requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            values: 
            - Large
            - Medium
```

# Schedule Pods: Based on Resources Requirements
---

## Resource Category
---

* CPU
	* Lowest Number: **0.1 = 100milli** 
	* Lowest Number with Unit: **1m = 1milli**
	* 1 CPU = **1000milli** = **1 AWS vCPU/Azure Core/GCP Core/HyperThread** 
	* When pod / node try to exceeds CPU limit, system will **throttle the usage of CPU** and don't let it exceed out, pod still running
* Memory (Mem)
	* 1K = 1 KiloByte = 1000 Bytes
	* 1M = 1 MegaByte = 1,000,000 Bytes
	* 1G = 1 GigaByte = 1,000,000,000 Bytes
	* 1Ki = 1 KibiByte = 1024 Bytes
	* 1Mi = 1 MebiByte = 1,048,576 Bytes
	* 1Gi = 1 GibiByte = 1,073,741824 Bytes
	* When pod / node try to exceed Memory Limit, it can run but **pod will be terminated** if it exceeds constantly due to `OOM (Out of Memory)`

## Default Behavior In Multiple-Node Cluster
---

By default, there are **no resources and limits** for nodes in cluster. `Kube-Scheduler` will distribute pod to node that have most sufficient amount of resources.

If there's no sufficient resources in all nodes in cluster, the pod will be in `pending` state due to insufficient cpu / memory.

## Define Resources Requirements Per Pod
---

### Define it in Pod Settings
---

* Can define following attributes in `spec.containers.resources` in following attributes:
	* requests: describes the **minimum** amount of compute resources required. It can't exceed limits and will be set as equilavent to limits if unspecified.
	* limits: describes the **maximum** amount of compute resources allowed.
* Best Practice: **Setting up requests but no limits**, so that when one pod has been with sufficient usage they can increase the unit for other cycle as well.
* Examples
```
apiVersion: v1

kind: Pod

metadata:

  name: nginx1

  labels:

    env: production

spec:

  containers:

  - name: nginx1

    image: nginx
    
  resources:
  
	  requests:
	  
		  memory: "4Gi"
		  
		  cpu: 2
		  
	  limits:
	  
		  memory: "8Gi"
		  
		  cpu: 4
```

### Setting Defaults for All Pods
---

To set up default resource requirements for all pods, `LimitRange` could be used as an example to set up at **namespace** level.

Examples:
```
apiVersion: v1

kind: LimitRange

metadata:

	name: resource-constraint
	
spec:

	limits: 
	
	- default / max: (limit)
	  cpu: 1m (or memory: 1Gi)
	  
	  defaultRequest / min: (Request)
	  cpu: 1m (or memory: 1Gi)
	  
	  type: Container
```


## Define Resources Requirements At Node / Cluster Level
---

To restrict the resource requirements for all pods in Cluster / all nodes, we can define it in `resourceQuota` at **namespace** level

Example:
```
apiVersion: v1

kind: ResourceQuota

metadata:

	name: my-resource-quota
	
spec:

	hard:
	
		requests.cpu: 4
		
		requests.memory: 4Gi
		
		limits.cpu: 10
		
		limits.memory: 10Gi
```

