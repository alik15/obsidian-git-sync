Pod to pod 
container to pod 
pod to service 
node to node 




# Kubernetes Networking Rules 

-  All Pods can communicate with each other on all Nodes 
- Agents on a Node can communicate with all Pods on that Node ( Agent means service)
- No network address translation (each pods get own IP address)


# Three main networks 
1. Node Network, connected through static IP addresses 
2. Pod Network 
3. Cluster Network used by services, allotted by default


A very basic intro to the setup 

![[Pasted image 20230809135018.png]]

Here we have 2 nodes setup. Each node as a few pods (red outline) with containers running in them. The pods can communicate within each other through a bridge the are also connected to the linux kernel if they want to communicate with the outside. 
If a pod wants to communicate with a pod in _another_ node, it has to use a layer2, 3 overlay 
this is provided by:

1. [[Container Network Interface#1. Flannel]]
2. [[Container Network Interface#2. Calico]]



### The Pod  Network


A CNI is used to provide the ip address from a network range 
























https://www.redhat.com/sysadmin/kubernetes-pods-communicate-nodes
https://medium.com/@extio/mastering-kubernetes-pod-to-pod-communication-a-comprehensive-guide-46832b30556b
