## Pods
---

* Definition: Describe & Create Pods In Kubernetes
* Sample YAML file 
```
apiVersion: v1

kind: Pod

metadata:

  name: nginx

  labels:

    env: production

spec:

  containers:

  - name: nginx

    image: nginx
```

## ReplicaSets
---

* Definition: Set up a list of pods with high availability and strategy of load balancing, scale up & down for pods in one or multiple nodes
* Sample File
```
apiVersion: apps/v1

kind: ReplicaSet

metadata:

  name: myapp-replicaset

  labels:

    app: myapp

spec:

  selector:

    matchLabels: -- This is to exclude other containers in the same node

      app: myapp

  replicas: 3 --This is to set the desired containers we wanna create

  template:

    metadata:

      name: nginx

      labels:

        app: myapp

    spec:

      containers:

      - name: nginx

        image: nginx
```

## Deployments
---

* Definition: A kind of replicas that allows the users to upgrade / rollback the pods seamlessly (Rolling Updates)
* Sample file: 
```
apiVersion: apps/v1

kind: Deployment

metadata:

  name: myapp-deployment

  labels:

    app: myapp

    tier: frontend

spec:

  selector:

    matchLabels:

      app: myapp

  replicas: 4

  template:

    metadata:

      name: nginx

      labels:

        app: myapp

    spec:

      containers:

      - name: nginx

        image: nginx
```