Pod to pod 
container to pod 
pod to service 
node to node 




There are 3 main rules Kubernetes imposes on networking
-  All Pods can communicate with each other on all Nodes 
- Agents on a Node can communicate with all Pods on that Node ( Agent means service)
- No network address translation (each pods get own IP address)

Three main networks 
1. Node Network, connected through static IP addresses 
2. Pod Network 

### The Pod  Network


