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


## Levels of DNS
### Root Name Server
- It is the official contact of last resort by the name servers that cannot resolve the name
### TLD
responsible for .org, .com, .net etc domains

### Authoritative DNS Servers
- organizations own DNS Servers providing authoritative hostname to IP mappings for organization's named hosts
- maintained by organization or service provider
### Local DNS Name Servers
- Queries are first sent to the local DNS Server, they check the local cache. If they find nothing they forward the request to the DNS hierarchy 


## DNS Name Resolution
### Iterated Query
![[Pasted image 20251013103319.png]]

### Recursive Query
![[Pasted image 20251013103358.png]]

## Cached DNS Information
Once a name server learns of a query, it is cached. This:
- Improves response times
- these entries disappears after some time 
cached entries may become out of date after some time 

## DNS Records
### Resource Record format
```
(name, value, type, ttl)
```
### Types of Resource Records

| Resource Record Type | Name                                           | Value                                         |
| -------------------- | ---------------------------------------------- | --------------------------------------------- |
| A                    | hostname                                       | IP Address                                    |
| CNAME                | Alias Name                                     | Canonical Name                                |
| MX                   | contains the name of the <br>admin@example.com | domain of the mail server<br>mail.example.com |
| NS                   | domain                                         | hostname of authoritative name server         |


## DNS Protocol Message
The message header of the DNS Protocol message 
- Identification: uses 16 bits for the query and reply
- Flags
	- query or reply
	- recursion desired
	- recursion available
	- reply is authoritative

## Getting your information into DNS
- First register your name with the DNS Registrar
	- Provide names, IP Addresses of authoritative name server(primary and secondary)
	- Registrar will insert NS, RRs into a .com TLD server
- Create an Authoritative Server to reply locally with the IP address 

## DNS Security
### DDoS Attack
#### Bombard Root servers
- Not very effective due to:
	- Traffic Filtering
	- other DNS servers have cached IPs of the TLD servers allowing them to bypass the root server
#### Bombard TLD servers
- Potentially more dangerous

### Spoofing Attacks
- Intercept the DNS queries with  bogus replies
- DNS cache poisoning