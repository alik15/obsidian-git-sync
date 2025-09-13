sudo find / -type f  -size +100M


useful vim things

curl -X GET "http://<elasticsearch-url>/_cat/indices/kube-system?v"
find . -type d -name "mysql"
who -b
uptime

git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install


scp file.txt remote_username@remote_host_ip:/remote/directory



CONTAINERD_ADDRESS=/run/k3s/containerd/containerd.sock /var/lib/rancher/rke2/bin/ctr -n k8s.io i export /tmp/routing-engine.tar gitimages.expertflow.com/cim/media-routing-engine/build:4.5.2_f-CIM-HOTFIX-fdef9e8e0dee4ff66af7ef168e54b4a51bcdd079

 CONTAINERD_ADDRESS=/run/k3s/containerd/containerd.sock /var/lib/rancher/rke2/bin/ctr  -n k8s.io i pull -u efcx:RecRpsuH34yqp56YRFUb  gitimages.expertflow.com/cim/unified-agent:4.5.6


 CONTAINERD_ADDRESS=/run/k3s/containerd/containerd.sock /var/lib/rancher/rke2/bin/ctr -n k8s.io i import /tmp/rasa_duckling_0.2.0.2.tar



sudo /usr/local/bin/rke2 server --debug


https://gist.github.com/superseb/3b78f47989e0dbc1295486c186e944bf#crictl

https://github.com/containerd/containerd/discussions/10053


>"C:\Program Files\Oracle\VirtualBox\VBoxManage.exe" modifymedium disk "F:\arch\arch.vdi" --resize 40000

Flush all IPs from the NIC
sudo ip addr flush dev enp3s0

# Flush all routes
sudo ip route flush dev enp3s0

At this point the NIC has no IP and no routes.
📡 Get a new IP via DHCP

sudo dhclient -r enp3s0   # release old lease
sudo dhclient enp3s0      # request new lease

Check:

ip addr show dev enp3s0
ip route
