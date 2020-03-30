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

* Create node-version deployment 
```bash
kubectl apply -f deployment.yml
```

* Create deployment 
```bash
kubectl get deployment
```

* Check Rollout status
```bash
kubectl rollout status deployment.apps/node-version-deployment
```

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

* Get pod logs
```bash
kubectl logs -f $POD_NAME
```

* Exec into pod 
```bash
kubectl exec -it $POD_NAME bash
```

* Let’s update the node-version Pods to use the jainishshah17/node-version:2.2.14 image instead of the jainishshah17/node-version:2.2.13 image.
```bash
kubectset image deployment.apps/node-version-deployment node-version=jainishshah17/node-version:2.2.14 --record
```

* Check Rollout status
```bash
kubectl rollout status deployment.apps/node-version-deployment
```

* Check rollout history
```bash
kubectl rollout history deployment.apps/node-version-deployment
```

* Delete deployment 
```bash
kubectl delete -f deployment.yml
```
