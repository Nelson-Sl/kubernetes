
## The mechanism of Kubectl apply
---

* It will decide the changes from local file, last applied configuration and live object configuration file. 
* When a setting has been deleted from local file but still exists in last applied configuration, that means this has been deleted last time in local file and should be updated to live object configuration file, then it will update the last applied configuration as well.