* **kubectl run [image-name] --image=[image-name]**: deploy an application / node to cluster with pod created
	* **-o yaml > [output-path]**: output as yaml
	* **--dryrun=client**: don't create the resource actually, use along with **-o=yaml** when we just want to output **yaml**
* **kubectl create deployment**: deploy an application / node to cluster within a special pod
* **kubectl cluster-info**: Get metadata / information about cluster
* **kubectl get nodes**: Get list of all nodes inside a container
	* **-o wide**: list all nodes with more details
* **kubectl get pods**: Get list of pods running in all nodes of a cluster
	* **-o wide**: list all pods with more details
	* **--all-namespaces (-A)**: list all pods with all namespaces
* **kubectl describe pod [pod-name]**: Get metadata / information about pod itself
* **kubectl delete pod [pod-name]**: Delete created / running pod
* **kubectl edit pod [pod-name]**: Edit pod service using default editor
	* **-o yaml**: output as YAML file
* **kubectl version**: Check version of Kubernetes
* **kubectl get all**: Get all created resources of kubernetes 
* **kubectl delete -f .**: Delete all created resources in the folder

### Related to ReplicaSet
---

* **kubectl create -f [ReplicaSet YAML]**: create replicaSet based on YAML file
	* -**-image=[image-name]**: create replicaset based on image directly
	* **-o yaml > [output-path]**: output as yaml
	* **--dryrun=client**: don't create the resource actually, use along with **-o=yaml** when we just want to output yaml
	* **--namespce=[namespace]**: create in specific namespace
* **kubectl get replicaset**: Get the information of ReplicaSet
* **kubectl delete replicaset [ReplicaSet name]**: Delete created ReplicaSet
* **kubectl replace -f [ReplicaSet YAML]**: Update ReplicaSet based on YAML file
* **kubectl scale --replicas = 6 -f [ReplicaSet YAML]**: Scale up ReplicaSet to 6 instances and update YAML file correspondingly
* k**ubectl describe replicaset [ReplicaSet name]**: Showing the metadata / information about replicaset
* **kubectl edit replicaset [ReplicaSet name]**: Editing the YAML template file of ReplicaSet

### Related to Deployment
---
* **kubectl create -f [Deployment YAML]**: create Deployment based on YAML file
* **kubectl get deployments**: Get the information of Deployments 
	* Can also use **kubectl delete replicaset [ReplicaSet name]** to check the status as well
* k**ubectl describe deployment [Deployment name]**: Showing the metadata / information about deployment itself
* **kubectl edit deployment [Deployment name]**: Editing the YAML template file of deployment
* **kubectl delete deployment [Deployment name]**: Delete created deployments
* **kubectl rollout status** **[Deployment name]**: check the rollout status of deployment
* **kubectl rollout history [Deployment name]**: check the rollout history of deployment and causing steps
* **kubectl rollout undo deployment [Deployment name]**: Rollback the deployment rollout / update
* **kubectl apply -f [Deployment YAML]** / **kubectl set image [Deployment name] **[configuration name]** : Change the configuration of Deployment
* **kubectl scale deployment [Deployment name] --replicas = [number]**: Scale up & Down the replica of specific deployment

## Related to Namespace
---
* **kubectl create -f [Namespace YAML]**: create namespace based on YAML file
* **kubectl create -f [ResourceQuota YAML]**: create resource quota for namespace in YAML file
* **kubectl create namespace [Namespace name]**: create namespace in one command
* **kubectl config set-context $(kubectl config current-context) --namepace=[Namespace name]**: set default namespace in k8s cluster