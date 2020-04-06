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

* Create index.html ConfigMap 
```bash
kubectl apply -f configmap-literal.yml
kubectl apply -f configmap-file.yml
```

* Create nginx deployment 
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
export NODE_PORT=$(kubectl get -o jsonpath="{.spec.ports[0].nodePort}" services nginx-service)

export NODE_IP=$(minikube ip)

echo http://$NODE_IP:$NODE_PORT/
```

Open printed URL in browser to access nginx application 

* Check pods
```bash
kubectl get pods
```

* Describe deployment
```bash
kubectl describe deployment nginx-deployment
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
# To print value of file from configMap
cat /usr/share/nginx/html/index.html
# To print value of variable from configMap
echo $AUTHOR
exit
```

* Delete service 
```bash
kubectl delete -f service.yml
```

* Delete deployment 
```bash
kubectl delete -f deployment.yml
```

* Delete configMap 
```bash
kubectl delete -f configmap-literal.yml
kubectl delete -f configmap-file.yml
```
