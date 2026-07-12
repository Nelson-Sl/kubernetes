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

