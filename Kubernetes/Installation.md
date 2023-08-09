



Prerequisites
you need the following packages/tools installed 

docker 
kube

## 1.1 Specifications 

Hardware: 4 cpus 4GBs 75GBs

OS: CentOS Linux release 7.9.2009 (Core)

Kubernetes: 1.27 

Docker: 24.0.4

Cli:


















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