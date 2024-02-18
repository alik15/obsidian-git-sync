

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




# Other things 
	https://devhints.io/vim