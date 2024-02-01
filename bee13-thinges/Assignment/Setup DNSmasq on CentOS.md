
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
run `ls` to list all the files and directories. Now you have to find the file which has the prefix 
`ifcfg` (meaning interface configuration) followed by the network device

now we can edit the file in the following ways:
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

 we will also add an upstream DNS server we have talked about earlier. 

```bash
# Google's nameservers
server=8.8.8.8
server=8.8.4.4
```

since there is no place defined in the files earlier we can add them at a position like the following:
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

### add test ips to your DNS server

add an ip - name pair in your `/etc/hosts` to test things. I have added the following:

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

run the following command 

```bash 
dig webserver1 +short 
```
![[Pasted image 20240130223052.png]]


## testing the dns from windows

### IP-lookup:

![[Pasted image 20240130223520.png]]

### Reverse IP lookup
![[Pasted image 20240130223556.png]]

wait, if your vms are on a private :LAN, how does windows find our dns servers ip?

# Making the dns server the default dns server for your private lan network

now we will make a smal

# 1.7 References
https://phoenixnap.com/kb/configure-centos-network-settings
https://www.tecmint.com/setup-a-dns-dhcp-server-using-dnsmasq-on-centos-rhel/

