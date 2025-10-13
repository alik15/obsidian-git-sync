- It is an application layer protocol where hosts communicate to resolve the names 
- This keeps the complexity at the edge of the network 

## DNS Services
- hostname to IP address translation
- hostname aliasing; maps canonical address to alias names
- mail server aliasing 
- load name distribution by replicating web servers since many IP addresses corresponds to one name.

## Why dont we centralize the DNS
- single point of failure 
- High traffic volume so might cause network congestion
- The location might be too far away from other clients
- might cause problems when having to maintain the server
- This solution does not scale

