
### 1.1 Install DNSmasq

**1.** The **dnsmasq** package is available in the default repositories and can be easily installed using the [YUM package manager](https://www.tecmint.com/20-linux-yum-yellowdog-updater-modified-commands-for-package-mangement/) as shown.

```shell
yum install dnsmasq
``` 

![[Install-dnsmasq-in-CentOS.png]]

### 1.2 Start DNSmasq service

```shell
systemctl start dnsmasq
systemctl enable dnsmasq
systemctl status dnsmasq
```


### 1.3 Configure DNSmasq file

```shell
vi /etc/dnsmasq.conf
```
Find out your ethernet interface 
```shell 
nmcli
```


![[Pasted image 20230831124157.png]]

In my case it is ```enp0s3```

#### 1.3.1 Change the following lines 

```
listen-address= ::1,127.0.0.1,{your vms ip address}

interface = enp0s3 (add your own ethernet interface)

port = 53
```



#### 1.3.2 Uncomment the following lines 
```
expand-hosts
domain-needed 
bogus-priv
expand-hosts
```

#### 1.3.3 Test dnsmasq server 
```shell
dnsmasq --test
```
#### 1.3.4 change your /etc/resolv.conf file 

remove/comment the 8.8.8.8 line and add

```
nameserver 127.0.0.1
```


The /etc/resolv.conf file is maintained by a local daemon especially the NetworkManager, therefore any user-made changes will be overwritten. To prevent this, write-protect it by setting the immutable file attribute (disabling write access to the file) using the chattr command as shown

```shell 
chattr +i /etc/resolv.conf
lsattr /etc/resolv.conf
```

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

