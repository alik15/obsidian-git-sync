



Prerequisites
you need the following packages/tools installed 

docker 
kube

## 1.1 Specifications 

Hardware: 4 cpus 4GBs 75GBs

OS: CentOS Linux release 7.9.2009 (Core)

Kubernetes: 1.27 

Docker: 24.0.4

CNI:Calico 




## 2.1 Installing Kubernetes 


> [!info]
> These steps need to be performed on all nodes, master and the two worker 
### 2.1.1 Configure Kubernetes Repository

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

#### 2.1.1.1 Installing docker as runtime 

Uninstalling any older variant
```bash
sudo yum remove docker docker-client-latest docker-common docker-latest-logrotate docker-engine
```


Installing config manager
```bash
sudo yum install -y yum-utils
```

setting up docker repository 

installing docker 


adding repo


sudo yum install -y yum-utils


sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo


sudo yum install -y docker-ce docker-ce-cli containerd.io




sudo systemctl daemon-reload
sudo systemctl restart docker  
sudo systemctl enable docker 
sudo systemctl status docker

### 2.1.2 Set Hostname on Nodes

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

###  2.1.3 Configure Firewall


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



### 2.1.4 Update Iptables Settings

Set the **`net.bridge.bridge-nf-call-iptables`** to '1' in your sysctl config file. This ensures that packets are properly processed by IP tables during filtering and port forwarding.

```
cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
```



### 2.1.5 Disable SELinux

The containers need to access the host filesystem. SELinux needs to be set to permissive mode, which effectively disables its security functions.

Use following commands to [disable SELinux](https://phoenixnap.com/kb/how-to-disable-selinux-on-centos-7):

```
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```


### 2.1.6 Disable SWAP

Lastly, we need to disable SWAP to enable the kubelet to work properly:

```
sudo sed -i '/swap/d' /etc/fstab
sudo swapoff -a
```







## 3.1 Deploying the Cluster

>[!Warning]
>Only run the following command on the master node


### 3.1.1 Create Cluster with kubeadm


```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```


if you encounter the following error:
![[Pasted image 20230818150110.png]]

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






### Manage Cluster as Regular User

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```



### Installing Calico as a CNI 

- Install the Tigera Calico operator and custom resource definitions.

```bash
   kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/tigera-operator.yaml
  ```

 Install Calico by creating the necessary custom resource.

 ```bash 
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/custom-resources.yaml
 ```

![[Pasted image 20230818151647.png]]















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