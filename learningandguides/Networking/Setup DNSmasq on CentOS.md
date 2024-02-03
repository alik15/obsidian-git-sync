
### Update centos

```bash 
sudo yum update
```

## Configuring a Static IP 
first we find the network interface we want to change by running:
```bash 
nmcli d
```

this should display all your network devices as shown below:
![[Pasted image 20240127120531.png]]

also figure out your ip address by running
```bash 
hostname -I
```

this should list 2 different ip addresses 
one of them is an ipv4 and the second one is an ipv6
we ill only be dealing with ipv4 so only select that 

since our virtual machine is connected to a virtual ethernet, is shows up as ethernet type. 

Now we have to edit some of the device's properties to do that we will go to the following directory:

```bash 
cd /etc/sysconfig/network-scripts

```
run `ls` to list all the files and directories 

now we can edit the file in the following ways:
**`BOOTPROTO`** to "static" and **`ONBOOT`** to "yes" if it isn't already set that way

 
### 1.1 Install DNSmasq

**1.** The **dnsmasq** package is available in the default repositories and can be easily installed using the [YUM package manager](https://www.tecmint.com/20-linux-yum-yellowdog-updater-modified-commands-for-package-mangement/) as shown.

```shell
sudo yum install dnsmasq
``` 

![[Install-dnsmasq-in-CentOS.png]]

### 1.2 Start DNSmasq service

```shell
systemctl start dnsmasq
systemctl enable dnsmasq
systemctl status dnsmasq
```


### 1.3 Configure DNSmasq file


Find out your ethernet interface 
```shell 
nmcli
```


![[Pasted image 20230831124157.png]]

In my case it is ```enp0s3```

```shell
vi /etc/dnsmasq.conf
```

#### 1.3.1 Change the following lines 

```
listen-address= ::1,127.0.0.1,{your vms ip address}

interface = enp0s3 (add your own ethernet interface)

port = 53
```

you can search for these in the following in vi by 
`/` followed by the name you are searching for the name 
you can scroll through the different instances of the name using `n` or backwards with `N`



#### 1.3.2 Uncomment the following lines 
```
expand-hosts
domain-needed 
bogus-priv
```

#### 1.3.3 Test dnsmasq syntax check 
```shell
dnsmasq --test
```
![[Pasted image 20240127151749.png]]
if there is a problem with the syntax, the test will tell you the line at which the syntax error has occurred. You can go to that line by either opening vi to that specific line or by opening the file in vi and pressing 
line number + down key

#### 1.3.4 change your host file 

```
vi /etc/resolv.conf
```
192.168.18.1
remove/comment the 8.8.8.8 line and add

```
nameserver 127.0.0.1
```


The /etc/resolv.conf file is maintained by a local daemon especially the NetworkManager, therefore any user-made changes will be overwritten. To prevent this, write-protect it by setting the immutable file attribute (disabling write access to the file) using the chattr command as shown

```shell 
chattr +i /etc/resolv.conf
lsattr /etc/resolv.conf
```

![[Pasted image 20240130212655.png]]

it should output the following line if it has worked

if you made an error in the resolv.conf file and want to change it again, you will have to remove the write-protect

### 1.4 Restart your DNSmasq service 
```bash
systemctl restart dnsmasq
```


## 1.5 Add DNS services in firewalld

 ```bash 
 firewall-cmd --add-service=dns --permanent
 firewall-cmd --add-service=dhcp --permanent
 firewall-cmd --reload
```


## 1.6 Testing your DNS server  

add an ip - name pair in your /etc/hosts

```bash 
yum install bind-utils
```

run the following command 

```bash 
dig webserver1 +short 
```

## 1.7 References
https://phoenixnap.com/kb/configure-centos-network-settings
https://www.tecmint.com/setup-a-dns-dhcp-server-using-dnsmasq-on-centos-rhel/

