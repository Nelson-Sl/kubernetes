# Labels
---

* `kubectl get pods --selector key=value` : Getting pods which matches the label key and value
* `kubectl label nodes <node-name> <label-key>=<label-value>`: adding lables for nodes

# Taints & Tolerations
---

* `kubectl taint nodes <node-name> key=value:taint-effect`: adding taints for nodes
* `kubectl taint nodes <node-name> key=value:taint-effect-`: untaint / delete taint on nodes
* `kubectl describe node <node-name> |grep Taint`: checking taint rules in node

