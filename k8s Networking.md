
# Single Node Network
---

* One External IP for communication with other system (E.g. 192.168.1.2)
* One Internal IP Group for internal communication between nodes (E.g. 10.240.0.0). And all the the nodes will be assigned with IP in this group (E.g. 10.240.0.2, 10.240.0.3)

# Cluster Network
---
* Inside the node they might be assigned to the same IP Group, therefore causing conflict when communicating
* Using routing tools / solution like Cisco, VMWare NSX to manage the network and routing IPs