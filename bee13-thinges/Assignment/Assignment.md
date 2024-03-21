
# Introduction 
 In this assignment you will learn about DNS servers and then how to setup a DNS server yourself. We will also learn how to create virtual machines and use it as a DNS server for private network. 

>[!info]
>Make sure that the network configuration on your virtual machine is set to NAT if you are on the NUST eduroam network, otherwise if its set to Bridged Adapter your VM will try to obtain an ip address for itself which doesn't work in eduroam. 
# DNS servers

DNS stands for Domain Name System

DNS Servers are the core of every network, they are used by every network to figure out what the  "location" of a server is. Every time you perform a search or click on a link, a DNS server is queried. 

This can be analogized as the following:
When Sir Hassaan wants to talk to Sir Ali Hassan who teacher MCS, he doesn't remember his number since he has to talk to hundreds of people everyday and cant remember all those numbers. Thus he keeps a list and "looks up" Sir Ali's number. This is essentially what DNS servers do. They keep a list of website names and their IP addresses and whenever a client queries the website name, the DNS server responds to the query with the IP address


## Public DNS server vs Private DNS servers
Public DNS servers are servers with a public IP address and can be queried by anyone. They keep a record of all public websites and respond to queries to all clients. 

>[!faq]
>If we have public dns servers to do all the work for us why do we need a private dns server?

Private DNS servers are usually on a private network, they are used to handle IP addresses that  in the private network range. If a company is hosting a website for its internal operations, think a bakery or a shop that needs to provide its workers with an interface to access the prices database etc. but doesn't want it open to public for security reasons, they would use a private DNS server hosted locally.
Thats not all private DNS servers are used for, they can also be used to provide caching abilities for faster lookups


There are 2 popular types of DNS servers 
1. Dnsmasq
2. BIND

We will be setting up a Dnsmasq server since its easier to setup and lightweight as compared to BIND.


Read the following RFC to understand what private IP's are 
https://www.rfc-editor.org/rfc/rfc1918

Read the following RFC to understand how DNS servers work in more detail
https://www.rfc-editor.org/rfc/rfc1035

# Setting up the DNS server
# Update centos

```bash 
sudo yum update
```

# Configuring a Static IP 

### Figuring out Network Interface
First we find the network interface we want to change by running:
```bash 
nmcli d
```

This should display all your network devices as shown below:
![[Pasted image 20240127120531.png]]

Since our virtual machine is connected to a virtual ethernet, is shows up as ethernet type. 
### Finding out IP address
 Figure out your IP address by running: 
```bash 
hostname -I
```

This should list 2 different ip addresses, one of them is an ipv4 and the second one is an ipv6.
we ill only be dealing with ipv4 (192.168.18.81):

![[Pasted image 20240130214828.png]]



### Editing network-scripts file
Now we have to edit some of the device's properties to do that we will go to the following directory:

```bash 
cd /etc/sysconfig/network-scripts

```
Run `ls` to list all the files and directories. Now you have to find the file which has the prefix 
`ifcfg` (meaning interface configuration) followed by the network device

Now we can edit the file in the following ways:
1. Vi/Vim
2. Nano
While this guide uses vi, you can choose the second one if you want. 

**`BOOTPROTO`** to "static" and **`ONBOOT`** to "yes" if it isn't already set that way
![[Pasted image 20240130215425.png]]
 
# Setting up DNSmasq 

### Download DNSmasq

 The **dnsmasq** package is available in the default repositories and can be easily installed using the [YUM package manager](https://www.tecmint.com/20-linux-yum-yellowdog-updater-modified-commands-for-package-mangement/) as shown.

```shell
sudo yum install dnsmasq
``` 

![[Install-dnsmasq-in-CentOS.png]]

### Start DNSmasq service

```shell
systemctl start dnsmasq
systemctl enable dnsmasq
systemctl status dnsmasq
```


### Configure DNSmasq file
We will now be be editing out dnsmasq file. Ensure that you write down your ip address and your network device name we found at the very start as it will make things easier for you.

Open up the file:

```shell
vi /etc/dnsmasq.conf
```

### Change the following lines 

```
listen-address= ::1,127.0.0.1,192.168.18.81 add your vm's ip address)

interface = enp0s3 (add your own ethernet interface)

port = 53
```

 We will also add an upstream DNS server we have talked about earlier. 

```bash
# Google's nameservers
server=8.8.8.8
server=8.8.4.4
```

Since there is no place defined in the files earlier we can add them at a position like the following:
![[Pasted image 20240130221642.png]]


You can search for these in the file using vi


###  Uncomment the following lines 

```
expand-hosts
domain-needed 
```

###  Test DNSmasq syntax check 

```shell
dnsmasq --test
```

![[Pasted image 20240127151749.png]]
if there is a problem with the syntax, the test will tell you the line at where the syntax error  occurred. You can go to that line by either opening vi to that specific line or by opening the file in vi and pressing 
line number + down key

### Changing host file 

```
vi /etc/resolv.conf
```


remove/comment the 8.8.8.8 the other lines and add

```
nameserver 127.0.0.1
```
![[Pasted image 20240130220626.png]]

The /etc/resolv.conf file is maintained by a local daemon in CentOS:NetworkManager, therefore any user-made changes will be overwritten when we want to apply them. To prevent this, write-protect it by making it immutable.

```shell 
chattr +i /etc/resolv.conf
lsattr /etc/resolv.conf
```

![[Pasted image 20240130212655.png]]

it should output the following line if it has worked

if you made an error in the resolv.conf file and want to change it again, you will have to remove the write-protect

### Add test ips to your DNS server

add an ip - name pair in your `/etc/hosts` to test things. I have added the following(last line, dont mess with the other lines):

![[Pasted image 20240130222109.png]]

This will be useful later.

### Restart your DNSmasq service 

```bash
systemctl restart dnsmasq
```

>[!warning]
>This single command isnt just for fluff, your DNS server settings will not update if you dont run the above command 


###  Add DNS services in firewalld

 ```bash 
 firewall-cmd --add-service=dns --permanent
 firewall-cmd --reload
```



## Testing your DNS server  

We will be testing and using our dns server in various ways

First install the following:

```bash 
yum install bind-utils
```

Run the following command 

```bash 
dig webserver1 +short 
```
![[Pasted image 20240130223052.png]]


## Testing the DNS from windows

### IP-lookup:

![[Pasted image 20240130223520.png]]

### Reverse IP lookup
![[Pasted image 20240130223556.png]]



# 1.7 References
https://phoenixnap.com/kb/configure-centos-network-settings
https://www.tecmint.com/setup-a-dns-dhcp-server-using-dnsmasq-on-centos-rhel/

