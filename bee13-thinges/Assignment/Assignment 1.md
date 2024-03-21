
# Introduction 
 In this assignment you will learn about DNS servers and then how to setup a DNS server yourself 
 we will also learn how to create virtual machines and use it as a DNS server for private network. 
 
# DNS servers

DNS stands for Domain Name System

DNS Servers are the core of every network, they are used by every network to figure out what the  "location" of a server is. Every time you perform a search or click on a link, a DNS server is queried. 

This can be analogised as the following:
for example when Sir Hassaan wants to talk to Sir Ali Hassan who teacher MCS, he doesnt remember his number since he has to talk to hundreds of people everyday and cant remember all those numbers. Thus he keeps a list and "looks up" Sir Ali's number. This is essentially what DNS servers do. They keep a list of website names and their IP addresses and whenever a client queries the website name, the DNS server responds to the query with the ip address


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
