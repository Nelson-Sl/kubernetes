# Category
---

* NodePort
	*  a **NodePort** is a service type that exposes a service to **external traffic** by opening a specific port on every Node (server) in the cluster.
	* Valid Range: **30000-32767**
	* Differences between Port, NodePort and targetPort
		* **Port (`port`):** The port exposed by the service _inside_ the cluster. Other pods use this port to communicate with the service.
		- **Target Port (`targetPort`):** The port the container is listening on. This is where the service sends traffic. If not specified, it defaults to the `port` value.
		- **Node Port (`nodePort`):** A port opened on _every_ node in the cluster (usually 30000-32767). It allows external traffic from outside the cluster to access the service![[Pasted image 20260613165944.png]]
* Cluster IP
	* It provides a **stable, internal IP address** that allows different components **within your cluster** (like a front-end web server and a back-end database) to communicate with each other.
* Load Balancer

