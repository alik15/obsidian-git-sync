

```bash
#using an imperative command to create a pod
kubectl run my-ubuntu-pod --image=ubuntu:latest --restart=Never --dry-run=client -o yaml > pod.yaml

#creating a yaml file of a running pod 
kubectl get pod <pod-name> -o yaml > pod-config.yaml


```
### Commands and EntryPoint in Docker 

### Commands and Args in Kubernetes 


### Env Variable 

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



### Secrets
not encrypted but encoded
very similar to config maps however they have some important differneces:
1. they are always stored in tmpfs(memory based storage instead of disk)
2. it is only sent to a node once a pod on that node asks for it
3. once a pod that depends on the secret gets deleted, the copy owned by kubernetes also gets deleted
4. 
```bash 
kubectl create secret generic 
<secret-name> --from-literal=<key>=<value>

kubectl create secret generic 
<secret-name> --from-file=file.txt

```


### Security Context 
used to give or remove privileges from the user

security context can be given at a pod level and at an image level 




### Service Accounts

#### user account
admin account 
dev account
#### service account
application accounts used by things like prometheus or jenkins


when a service account is created it also creates a service account token which has to be used  by external application
stored as secret object 



## Section 7: Services & Networking
### Services
enable connectivity among pods and for outside users
allows us to expose pods to the external env


Types
NodePort 
exposes a pod outside
range = 30000-32767

ClusterIP
LoadBalancer
