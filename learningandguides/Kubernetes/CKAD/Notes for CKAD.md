

## Syllabus
 - [ ] 20% - Application Design and Build
     - [ ] Define, build and modify container images
     - [ ] Understand Jobs and CronJobs
     - [ ] Understand multi-container Pod design  
     - [ ] patterns (e.g. sidecar, init and others)
     - [ ]  Utilize persistent and ephemeral volumes
 - [ ] 20% - Application Deployment
    - [ ] Use Kubernetes primitives to implement  common deployment strategies (e.g. blue/green or canary) 
- [ ] Understand Deployments and how to  perform rolling updates
    - [ ] Use the Helm package manager to deploy existing packages
- [ ] Application observability  and maintenance
	- [ ] Understand API deprecations
	- [ ] Implement probes and health checks
	- [ ] Use provided tools to monitor Kubernetes  applications
	- [ ]  Utilize container logs
	- [ ] Debugging in Kubernetes
 - [ ] 25% - Application Environment,  Configuration and Security
	- [ ] Discover and use resources that extend Kubernetes (CRD)
	- [ ] Understand authentication, authorization and admission control
	- [ ] Understanding and defining resource  requirements, limits and quotas
	- [ ] Understand ConfigMaps
	- [ ] Create & consume Secrets
	- [ ] Understand ServiceAccounts
	- [ ] Understand SecurityContexts
 - [ ] 20% - Services & Networking
	 - [ ] Demonstrate basic understanding of  NetworkPolicies
	 - [ ] Provide and troubleshoot access to  applications via services
	- [ ] Use Ingress rules to expose application
- [ ] Miscellaneous 
	- [ ] Kubeconfig
	- [ ] Role Based Access
	


Running commands from inside the container
```bash
k exec webapp -- cat /log/app.log
```


### Config Maps 

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

```YAML
 


```



### Security Context 
- Used to give or remove privileges from the user
- Security context can be given at a pod level and at an image level 
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


### Resources
 requests should go above limits 

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