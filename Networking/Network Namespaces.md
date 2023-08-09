- Works on the OS level? 
- Provide isolation to the container in terms of networking 
## An Analogy 

The network can be thought of as a house while namespaces can be thought of as rooms within the house

Each room provides: 
- privacy to each child
- each person can only see inside the room and cannot see outside
- as the network  manager you have visibility inside each room 


![[Pasted image 20230809115616.png]]

In the exact same way a container is isolated and doesn't see any other processes or any containers on the host, as far as the container is concerned it is on its own host and sees no other containers on here.

A process running inside the container will have different process IDs inside the container and on the host


![[Pasted image 20230809120140.png]]

Each host needs 
-  A NIC with an IP address
-  Routing table
-  ARP table 

In the same way a network namespace needs the exact same things which we can provide  virtually


#### Creating a  namespaces from the terminal

![[Pasted image 20230809121834.png]]


To list all network interfaces use 

```bash 
ip link
```


## Getting into the network namespace
use either of the following commands 

```bash 
sudo ip netns exec red ip link
```

```bash 
ip -n red ilnk
```

### Taking a look at ARP and Routing tables inside the host vs inside the container

![[Pasted image 20230809122925.png]]

As we can see, the host and container ARP/routing tables are different showing that the container cannot see the hosts ARP/routing tables


### Connecting Namespaces together using a pipe

#### Creating the pip/virtual cable and attaching it
We are essentially creating a virtual cable with interfaces on both ends

![[Pasted image 20230809130134.png]]


#### Assigning ip addresses to the namespaces
![[Pasted image 20230809130506.png]]



#### Bring up the interfaces 

![[Pasted image 20230809131422.png]]


#### Pinging one namespace from the other namespace 

use the command
```bash
sudo ip netns exec red ping 192.168.15.2
```



The following video can be used to continue the tutorial 
	https://youtu.be/j_UUnlVC2Ss

















what if the namespace ip address is the same as an actual ip address