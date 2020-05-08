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

* Enable ingress minikube addon
```bash
minikube addons enable ingress
```

* Create node-version deployment for V1 and V2
```bash
kubectl apply -f deployment-v1.yml
kubectl apply -f deployment-v2.yml
```

* Check deployment 
```bash
kubectl get deployment
```

* Create service for V1 and V2
```bash
kubectl apply -f service-v1.yml
kubectl apply -f service-v2.yml
```

* Check services 
```bash
kubectl get services
```

* Check pods
```bash
kubectl get pods
```

* Describe deployment
```bash
kubectl describe deployments node-version-v1-deployment
kubectl describe deployments node-version-v2-deployment
```

* Describe pod
```bash
kubectl describe pod $POD_NAME
```

* Create Ingress 
```bash
kubectl apply -f  ingress.yml
```

* Check Ingress 
```bash
kubectl get ingress
```

* Describe Ingress 
```bash
kubectl describe ingress node-version-ingress
```

* Add DNS entry in `hosts` file
```bash
echo "$(minikube ip)  v1.jainishshah.com v2.jainishshah.com" >> /etc/hosts
```

* Open following URLs to access version V1 and V2 of node-version application.
```bash
http://v1.jainishshah.com/
http://v2.jainishshah.com/
```

* Delete service 
```bash
kubectl delete -f ingress.yml
```

* Delete service 
```bash
kubectl delete -f service-v1.yml
kubectl delete -f service-v2.yml
```

* Delete deployment 
```bash
kubectl delete -f deployment-v1.yml
kubectl delete -f deployment-v2.yml
```
