# Introduction to kubernetes

## Install Minikube

* To check if virtualization is supported on macOS, run the following command on your terminal.

```bash
sysctl -a | grep -E --color 'machdep.cpu.features|VMX' 
```

If you see VMX in the output (should be colored), the VT-x feature is enabled in your machine.

* Install Minikube
The easiest way to install Minikube on macOS is using Homebrew:

```bash
brew install minikube
```

* Start minikube with hyperkit

```bash
minikube start --vm-driver=hyperkit
```

* Check the status of the cluster:

```bash
minikube status
```

* Check minikube version:

```bash
minikube version
```

* Enable minikube dashboard:

```bash
minikube dashboard
```

* Install kubectl:
```bash
brew install kubectl

kubectl version
```

* Cluster information:
```bash
kubectl cluster-info
```

* Enable metrics-server minikube addon
```bash
minikube addons enable metrics-server
```

* Create node-version deployment 
```bash
kubectl apply -f deployment.yml
```

* Check deployment 
```bash
kubectl get deployment
```

* Create service 
```bash
kubectl apply -f service.yml
```

* Check services 
```bash
kubectl get services
```

* Get Application URL
```bash
export NODE_PORT=$(kubectl get -o jsonpath="{.spec.ports[0].nodePort}" services node-version-service)

export NODE_IP=$(minikube ip)

echo http://$NODE_IP:$NODE_PORT/
```

Open printed URL in browser to access node-version application 

* Check pods
```bash
kubectl get pods
```

* Describe deployment
```bash
kubectl describe deployments node-version-deployment
```

* Describe pod
```bash
kubectl describe pod $POD_NAME
```

* Create Horizontal Pod Autoscaler
```bash
kubectl create -f hpa.yml
```

* Check Horizontal Pod Autoscaler
```bash
kubectl get hpa
```

* Let's try to increase cpu usage of deployment using running following commands:  
```bash
export NODE_PORT=$(kubectl get -o jsonpath="{.spec.ports[0].nodePort}" services node-version-service)

export NODE_IP=$(minikube ip)

while true; do curl http://$NODE_IP:$NODE_PORT/;done
```

* Check Horizontal Pod Autoscaler 
```bash
kubectl get hpa -w
```

**Note**: You will see numbers of replicas increasing/decreasing based on cpu usage.

* Delete Horizontal Pod Autoscaler  
```bash
kubectl delete -f hpa.yml
```

* Delete service 
```bash
kubectl delete -f service.yml
```

* Delete deployment 
```bash
kubectl delete -f deployment.yml
```
