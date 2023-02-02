# Kubernetes for beginners training

<br />

## Enable Kubernetes single-node cluster on Docker-Desktop 
* Kubernetes can be enabled from Kubernetes settings panel
* Check the Enable Kubernetes box and press Apply & Restart 
* (Optional) Check the Use Rosetta for x86/amd64 emulation on Apple Silicon box
* And Enable the virtualization framework in General panel then press Apply and Restart

<br />

## Get the Docker-Desktop node's ip address
```yaml
cat /etc/hosts
# To allow the same kube context to work on the host and the container:
127.0.0.1 kubernetes.docker.internal
```
<br />

## Multiple Deployments - Project description -
* Create a new deployment for our node.js application, named web
* This application contains two routes
```yaml
  '/'
  '/nginx'
```
* We are exposing this deployment using a service type LoadBalancer
* Using LoadBalancer will enable connecting using the LoadBalancer IP address
* Create another deployment that will run a standard nginx webserver
* We are exposing this deployment using a service type ClusterIP
* From one deployment we will connect to another, and we'll get a response from one of pods running nginx webserver

<br />

## Application manifest files
```yaml
/application/index.mjs
/application/package.json
/application/package-lock.json
```
<br />

## Docker manifest files
```yaml
/application/Dockerfile
```
<br />

## K8s manisfest files
```yaml
deployment/k8s-web-to-nginx.yaml
deployment/nginx.yaml
``` 

<br />

## Docker commands

<br />

### Build and push the image to Docker Hub
```yaml
docker build . -t alexjdevil/k8s-web-to-nginx:v1.0
docker push alexjdevil/k8s-web-to-nginx:v1.0
```
<br />

## K8s commands

<br />

### Get K8s cluster info
```yaml    
kubectl cluster-info
```
### Get basic info about K8s components
```yaml
kubectl get node
kubectl get pod
kubectl get svc
kubectl get all
kubectl delete all -all
```
<br />

### Get extended info about components
```yaml
kubectl get pod -o wide
kubectl get node -o wide
```

<br />

### Get detailed info about a specific component
```yaml
kubectl describe svc {svc-name}
kubectl describe pod {pod-name}
```
<br />

### Get application logs
```yaml
kubectl logs {pod-name}
```

<br />
<br />

## Deploy everything
* Deploy all deployments and services yaml's
```yaml
  kubectl apply -f ./deployment/k8s-web-to-nginx.yaml
  kubectl apply -f ./deployment/nginx.yaml
```
* Confirm the deployments, pods and services were created and started
```yaml
  kubectl get pods -n default
  kubectl get services -n default
```
* Check deployments and services details
```yaml
  kubectl describe deployment k8s-web-to-nginx
  kubectl describe deployment nginx
  kubectl describe service k8s-web-to-nginx
  kubectl describe service nginx 
```
* Access the Application using both endpoints
```yaml
  http://localhost:3333
  http://localhost:3333/nginx
```

<br />
<br />

## DNS or or name resolution of pods when querying inside the Kubernetes Cluster
* Query the service from one of the pods 
```yaml
  kubectl exec k8s-web-to-nginx-6f64c979c9-256wm -- nslookup nginx
```
* Compare the IP address by checking from outside the Kubernetes cluster
```yaml
  kubectl get services -o wide
```
* Another way of testing DNS by retrieving the contents of the webserver from inside any pod
```yaml
  kubectl exec k8s-web-to-nginx-6f64c979c9-vhf6n -- wget -qO- http://nginx
```

<br />
<br />

### Clean up everything
```yaml
  kubectl delete -f ./deployment/k8s-web-to-nginx.yaml
  kubectl delete -f ./deployment/nginx.yaml
```

<br />

## Stop or quit your Kubernetes cluster
* To pause or quit click on the Moby icon in Upper right corner and chose either pause or stop 

<br />
<br />

## Links
* k8s official documentation: https://kubernetes.io/docs/home/
* webapp code repo: https://github.com/AlexJDevil/k8s-web-to-nginx
* k8s-web-to-nginx image on Docker Hub: https://hub.docker.com/repository/docker/alexjdevil/k8s-web-to-nginx/general

