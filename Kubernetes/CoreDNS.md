## CoreDNS vs kube-DNS

- plugin architecture 
- simpler ( kubeDNS had 3 different executables and was written in C)
- customizable DNS entries in and out of the cluster domain 
- reduces resolution


# Introduction 

-  Fork of SkyDNS
-  Plugin architecture 
- DNS over TLS
- Support for Prometheus metrics 


## Core DNS as ClusterDNS [^1]

- Connected to the API of kubernetes where it watches services, pods and endpoints 

- Resolves any query that aims at another ip but if it cant resolve it internally then it upstreams it outside the cluster 
upstreams ip outside the cluster 

### Specifications in deployment file 
- larger the cluster larger the memory and cpu is needed (about 1k or 2k nodes)
- by default it is 70 MBs but the limit is 170 MBs for a 5k node cluster
- minimum 2 are always working 
- liveness probe: checks if the pod is working otherwise it is restarted using the scheduler 
-  there is a policy for each pod called dnsPolicy where they call on the CoreDNS as the name server, *except* the coreDNS( we dont want coreDNS calling itself to resolve)
























[^1]: can i use another dns as a coredns in clusterdns 







