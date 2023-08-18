



Prerequisites
you need the following packages/tools installed 



## 1 Specifications 


|            | Specifications                       |
| ---------- | ------------------------------------ |
| Hardware   | 4 cpus 4GBs 75GBs                    |
| Os         | CentOS Linux release 7.9.2009 (Core) |
| Kubelet |                      1.28                |
| Docker     |  24.0.5                |
| CNI        |      Calico                                |
| Containerd            |         1.6.22                           |







## 2 Installing Kubernetes 


> [!info]
> These steps need to be performed on all nodes, master and the two worker 
### 2.1 Configure Kubernetes Repository

Kubernetes packages are not available from official CentOS 7 repositories. This step needs to be performed on the Master Node, and each Worker Node you plan on utilizing for your container setup. Enter the following command to retrieve the Kubernetes repositories.

```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
```



```bash
sudo yum install -y kubelet kubeadm kubectl
```

![[Pasted image 20230818111023.png]]

```bash
systemctl enable kubelet 
systemctl start kubelet
```

#### 2.1.1 Installing docker as runtime 

Uninstalling any older variant you might have

```bash
sudo yum remove docker docker-client-latest docker-common docker-latest-logrotate docker-engine
```


Installing config manager

```bash
sudo yum install -y yum-utils
```


Adding docker repository 

```bash 
sudo yum install -y yum-utils


sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo


sudo yum install -y docker-ce docker-ce-cli containerd.io

```


#### Configuring Docker for Cgroup management


check if **/etc/docker/daemon.json** exists

if it doesnt, make it and paste the following inside 
```json
{
"exec-opts": ["native.cgroupdriver=systemd"],
"log-driver": "json-file",
"log-opts": { "max-size": "100m" },
"storage-driver": "overlay2"
 }
```

Now run the following commands 

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker  
sudo systemctl enable docker 
sudo systemctl status docker
```

### 2.2 Set Hostname on Nodes

To give a unique hostname to each of your nodes, use this command:

```
sudo hostnamectl set-hostname master-node
```

or

```
sudo hostnamectl set-hostname worker-node1
```

The master node is now named master-node, while the worker nodes are  named worker-node1 and worker-node2



Make a host entry or DNS record to resolve the hostname for all nodes:

```
sudo vi /etc/hosts
```

With the entries 

```
172.30.237.10  master-node
172.30.237.11  worker-node1
172.30.237.12  worker-node2
```

![[Pasted image 20230818110938.png]]

###  2.3 Configure Firewall


> [!warning]
> The following steps are only to be done on the Master Node


```bash
sudo firewall-cmd --permanent --add-port=6443/tcp
sudo firewall-cmd --permanent --add-port=2379-2380/tcp
sudo firewall-cmd --permanent --add-port=10250/tcp
sudo firewall-cmd --permanent --add-port=10251/tcp
sudo firewall-cmd --permanent --add-port=10252/tcp
sudo firewall-cmd --permanent --add-port=10255/tcp
sudo firewall-cmd --reload
```

![[Pasted image 20230818113259.png]]

>[!Warning]
>Add the following to the worker nodes only 


```bash
sudo firewall-cmd --permanent --add-port=10251/tcp
sudo firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --reload
```

![[Pasted image 20230818113339.png]]



### 2.4 Update Iptables Settings

Set the **`net.bridge.bridge-nf-call-iptables`** to '1' in your sysctl config file. This ensures that packets are properly processed by IP tables during filtering and port forwarding.

```
cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
```



### 2.5 Disable SELinux

The containers need to access the host filesystem. SELinux needs to be set to permissive mode, which effectively disables its security functions.

Use following commands to [disable SELinux](https://phoenixnap.com/kb/how-to-disable-selinux-on-centos-7):

```
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```


### 2.6 Disable SWAP

Lastly, we need to disable SWAP to enable the kubelet to work properly:

```
sudo sed -i '/swap/d' /etc/fstab
sudo swapoff -a
```







## 3 Deploying the Cluster

>[!Warning]
>Only run the following command on the master node


### 3.1 Create Cluster with kubeadm


```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```


if you encounter the following error:
![[Pasted image 20230818150110.png]]

Use the follow the follwing manual 
1. Correct configuration is specified for the endpoint "unix:///var/run/containerd/containerd.sock" and enabled the plugin instead of disabled.  
    Added below code in `/etc/containerd/config.toml`  
    [plugins."io.containerd.grpc.v1.cri".containerd]  
    endpoint = "unix:///var/run/containerd/containerd.sock"
    
2. systemctl restart containerd


you can explore it further from here
	https://github.com/containerd/containerd/issues/8139#issuecomment-1491536705


>[!info]
>paste the following command in other nodes to connect them to the master node


```bash
kubeadm join 172.30.237.10:6443 --token gmz1p5.qc38xzmxnkgxg3yr \
--discovery-token-ca-cert-hash sha256:204be7dc8f215e26b3b42409dc6596051cad0103f0789471210f93213d3d849f 

```






### 3.2 Manage Cluster as Regular User

>[!warning]
>run only on master node



```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```



### 3.3 Installing Calico as a CNI 



#### 3.3.1 Change local directory to the lab calico dir.

```bash 
   cd ~/agilitydocs/docs/class1/kubernetes/calico
```
 

#### 3.3.2 Download calico manifest

```bash
curl https://docs.projectcalico.org/manifests/calico.yaml -O
```

#### 3.3.3 Modify the manifest with proper POD CIDR

```bash 
vim calico.yaml
```



- Find the “CALICO__IPV4POOL_CIDR variable and uncomment the two lines as shown below. Replacing “192.168.0.0/16” with “10.244.0.0/16" or whatever ip range you used to intialise the cluster


![[Pasted image 20230818201014.png]]


#### 3.3.4 Start Calico on the cluster

```bash
kubectl apply -f calico.yaml
```


#### 3.3.5 Validate Calico pods are installed and running

```bash
kubectl get pods -n kube-system
```


### 3.4 Joining worker Nodes
#### 3.6 Kubeadm Join
- Use the kubeadm join you got from kubeadm init

- Do this on both nodes

![[Pasted image 20230818202426.png]]


### 4 Testing the Deployment 

```shell
kubectl get pods
```

```bash
kubectl get nodes
```
{screenshots of all these outputs }

```shell
kubectl apply -f https://k8s.io/examples/pods/simple-pod.yaml
```





Everytime you ssh into a node  and try using **kubectl** you will face the following error:

```bash 
Unable to connect to the server: tls: failed to verify certificate: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "kubernetes")

```

You can use the following command to get rid of it:
```bash
export KUBECONFIG=/etc/kubernetes/admin.conf
```


References 

Calico
https://clouddocs.f5.com/training/community/containers/html/appendix/appendix8/appendix8.html

Kubernetes 
https://phoenixnap.com/kb/how-to-install-kubernetes-on-centos

Docker 
https://www.youtube.com/watch?v=Ro2qeYeisZQ&t=239s


Pod yaml File 
```yaml

apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

```


Service Yaml File 

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```