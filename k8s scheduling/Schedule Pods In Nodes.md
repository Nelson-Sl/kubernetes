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