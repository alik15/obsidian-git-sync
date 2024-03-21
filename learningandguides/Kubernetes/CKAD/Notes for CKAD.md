

## Syllabus
 - [ ] 20% - Application Design and Build
     - [x] Define, build and modify container images
     - [x] Understand Jobs and CronJobs
     - [x] Understand multi-container Pod design  
     - [x] patterns (e.g. sidecar, init and others)
     - [x]  Utilize persistent and ephemeral volumes
 - [ ] 20% - Application Deployment
    - [x] Use Kubernetes primitives to implement  common deployment strategies (e.g. blue/green or canary) 
- [ ] Understand Deployments and how to  perform rolling updates
    - [x] Use the Helm package manager to deploy existing packages
- [ ] Application observability  and maintenance
	- [x] Understand API deprecations
	- [x] Implement probes and health checks
	- [x] Use provided tools to monitor Kubernetes  applications
	- [x]  Utilize container logs
	- [x] Debugging in Kubernetes
 - [ ] 25% - Application Environment,  Configuration and Security
	- [x] Discover and use resources that extend Kubernetes (CRD)
	- [x] Understand authentication, authorization and admission control
	- [x] Understanding and defining resource  requirements, limits and quotas
	- [x] Understand ConfigMaps
	- [x] Create & consume Secrets
	- [x] Understand ServiceAccounts
	- [x] Understand SecurityContexts
 - [ ] 20% - Services & Networking
	 - [x] Demonstrate basic understanding of  NetworkPolicies
	 - [x] Provide and troubleshoot access to  applications via services
	- [ ] Use Ingress rules to expose application
- [ ] Miscellaneous 
	- [x] Kubeconfig
	- [ ] Role Based Access
	


Running commands from inside the container
```bash
k exec webapp -- cat /log/app.log
```


--dry-run=client 
-o yaml
## Pods 
### Creating pods
```bash
k run <pod-name> --image <image> --port=<port-num> --label

```

```bash
--labels #only works with pods
```
## Deployment
multiple containers in a single pod only works with deployment but cant give new commands to this
## Labels
## Config Maps 

Contain config data as key val pair in a central space

#### Creating Configmap:

```bash
kubectl create configmap 
<config-map-name> --from-literal=<key>=<value> 
//can also be from a file 

app-config --from-file= config.properties

//multiple config maps 

kubectl create configmap webapp-config-map --from-literal=APP_COLOR=darkblue --from-literal=APP_OTHER=disregard
configmap/webapp-config-map created
```

#### Viewing Configmaps
```bash
kubectl get configmaps
```


## Security Context 
- Used to give or remove privileges from the user
- Security context can be given at a pod level and at an container level 
```yaml
spec:
  securityContext:
    runAsUser: 1001
  containers:
  -  image: ubuntu
     name: web
     command: ["sleep", "5000"]
     securityContext:
      runAsUser: 1002

  -  image: ubuntu
     name: sidecar
     command: ["sleep", "5000"]

```


- user 1001 is an example of a security context at a pod level, user 1002 is an example of running at  a container level

- If security context is not given in the container, the security context of the pod will be used

- If both security Contexts arent given, the container will run as root user



```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo-4
spec:
  containers:
  - name: sec-ctx-4
    image: gcr.io/google-samples/node-hello:1.0
    securityContext:
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]
```

- Capabilities are only to be added to a container and not a pod


## Resources
 requests should go above limits 
can only be added in a yaml
### Security 
KubeConfig 
by default kubernetes looks for the .kube file in 
the 
```bash
$HOME/.kube/config
```

the kubeconfig defines the following 
- clustrers
- users 
- contexts

the contexts is used to define what users can access what clusters
default context is defined in the kubeconfig file


#### View Current Context
```shell
kubectl config view
```

#### Changing Context


### Role Based access

#### Creating Roles
#### Creating Roles Bindings

#### Viewing Roles
#### Viewing Role Bindings

#### Checking Access
```bash
k auth can-i create deployment
```


## Persistent Volumes









# Other things 
	https://devhints.io/vim
https://www.civo.com/learn/tips-to-ace-cka-and-ckad-exams
https://www.infracloud.io/blogs/prepare-cka-ckad-certification/

# Documentation of mock and lightning lab attempts

## Lightning Lab 1 (attempt1)

percentage = 0 perc
things needed to be worked on
config maps, creation, mounting inside volumes
volumes
persistent volumes
labels from the commandline 
exposing objects on the commandline 


## Mock Exam 1 (attempt 1)

62 perc


Ingress 
configmaps from the commandline
env variables
commands in pods
storageclass ( just take a basic look at it)
security contexts
service ports 

What are the steps in configuring a volume for a pod?
- what is the path provided in the volume mount and what is the path provided in the volume 
- should the names of the volume and the volume mount be the same?
- what is the persistent volume and the volume inside the pod
- how do we mount config maps onto pods

volumes are attached/used in pods
volumes are _mounted_ in containers
we can change the underlying 

## Mock Exam 1 (attempt 2)
91 perc
mistakes: 
- typo in pod name

- problem with network policy since i didnt add the port and the correct label in the yaml file



# Things I need to revise
- [x] **deployment strategies** 
- [ ] **cronjob time format**
- [ ] **helm**
- [x] **authentication, authorization and admission controllers**
- [x] **roles/cluster roles**
- [ ] **Deploying Ingress Controller**
- [x] **KubeConfig**
- [x] Taints/Tolerations 
- [x] Node Affinity
- [x] Command and Args imperative commands ( is there a difference between them when completing a task)
- [ ] **SideCar**
- [ ] Interactive terminal, exec into a pod, bin/sh into a container
- [ ] Podman
- [x] multiple containers from the command line( works only for deployment)
- [ ] how to force create a yaml of an already running pod?
- [ ] how to force delete  a pod quickly
- [ ] alias in kubernetes
- [x] k expose
- [ ] how do roles and role bindings connect to contexts etc
