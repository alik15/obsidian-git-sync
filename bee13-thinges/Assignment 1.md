
[info]
READ THE FOLLOWING INSTRUCTIONS VERY CAREFULLY
- Ensure that you have your full name as your username in your virtual machine 
- ensure that you give your virtual machine enough harddisk otherwise if it fills up you will have to make another one or mount another harddisk 

# Pre-Requisites
setting up a centos vm
communicating virtual machines with each other locally
assigning an ip to a virutal machine in virtual box
# Introduction
 In this assignment you will learn about DNS servers and then how to setup a DNS server yourself 
 we will also learn how to create virtual machines and use it as a DNS server for private network. 
 




Dns Servers are the core of every network
public dns server
go to the following link to learn about public dns servers, however in short dns servers are the PA.
this can be analogised as the following:
for example when sir hassaan wants to talk to Sir Ali Hassan who teacher MCS, he doesnt rmbr his number. 
He will have to "lookup" his number from a list or try to 
why do we need privte dns servser? 
-  cache or faster lookups
- internal website 
- private LANs
- 

if we have public dns servers to do all the work for us why do we need a private dns server?f
There are 2 popular types of dns servers 
1. Dnsmasq
2. dig

# using the nslookup command

### running the command

### breaking down the response 




## using the vms to try to ping each other

in a private network the ip address assigned are in the range
-
-
as described in the following RFC standard 



microsoft recently had an incident where they accidently pushed a private ip address to the public dns server. After reading the above section you should know why that is an extremly ludicrous and costly thing to do






# Deliverables 
congrats you have gotten through this document and have hopefully completed the assignment yourself. Since there is no other way to prove that you have set up a dns server yourself you will have to provide screenshots, you will have to ensure that your screenshots contain the following:

your username shown on your cli
