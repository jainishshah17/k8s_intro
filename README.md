# Introduction to kubernetes

## Install minikube

* To check if virtulaization is supported on macOS, run the following command on your terminal.

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

* Create Nginx deployment
```bash
kubectl create deployment nginx --image=nginx:1.16.1
```

* Check Deployment
```bash
kubectl get deployments
```

* Check pods
```bash
kubectl get pods
```

* Describe pod
```bash
kubectl describe pod $POD_NAME
```

* Get pod logs
```bash
kubectl logs -f $POD_NAME
```

* Exec into pod 
```bash
kubectl exec -it $POD_NAME bash
```

* Create service:
```bash
kubectl expose deployment/nginx --type="NodePort" --port 80
```

* Get services:
```bash
kubectl get services
```

* Get minikube IP
```bash
minikube ip
```

* Get Replica sets
```bash
kubectl get rs
```

* Scaling a deployment up 
```bash
kubectl scale deployments/nginx --replicas=4
```

* Scaling a deployment down 
```bash
kubectl scale deployments/nginx --replicas=2
```

* Rolling Update image
```bash
kubectl set image deployments/nginx nginx=nginx:1.17.9
```
