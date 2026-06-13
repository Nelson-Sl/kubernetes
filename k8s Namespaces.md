## Definition (What)
---

* In Kubernetes, _namespaces_ provide a mechanism for **isolating groups of resources within a single cluster**. Names of resources need to be unique within a namespace, but not across namespaces. Namespace-based scoping is applicable only for namespaced [objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/#kubernetes-objects) _(e.g. Deployments, Services, etc.)_ and not for cluster-wide objects _(e.g. StorageClass, Nodes, PersistentVolumes, etc.)_.

## Why and When
---

* Namespace use when different resources should be isolated between each other. E.g. the resources in dev and prod

## Default DNS
---

* E.g. **db-service.dev.svc.cluster.local** is the DNS that created automatically for db-service in dev namespace for other namespace to visit. For this DNS:
	* **db-service** is the service name
	* **dev** is the namespace that the resource belongs to
	* **svc** is the subdomain name for service
	* **cluster.local** is the domain name of Kubernetes Cluster
