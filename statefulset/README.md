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

* Create node-version statefulset 
```bash
kubectl apply -f statefulset.yml
```

* Check statefulset 
```bash
kubectl get statefulset
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

* Check Rollout status
```bash
kubectl rollout status sts node-version-statefulset
```

* Check pods
```bash
kubectl get pods
```

* Describe statefulset
```bash
kubectl describe sts node-version-statefulset
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

* Let's scale node-version statefulSet to 5 replicas
```bash
kubectl scale sts node-version-statefulset --replicas=5
```

* Check Rollout status
```bash
kubectl rollout status sts node-version-statefulset
```

* Let’s update the node-version Pods to use the jainishshah17/node-version:2.3.1 image instead of the jainishshah17/node-version:2.3.2 image.
```bash
kubectl set image sts node-version-statefulset node-version=jainishshah17/node-version:2.3.2 --record
```

* Check Rollout history
```bash
kubectl rollout history sts node-version-statefulset
```

* Refresh application URL to see new version being deployed.
```bash
echo http://$NODE_IP:$NODE_PORT/
```

* Delete service 
```bash
kubectl delete -f service.yml
```

* Delete statefulset 
```bash
kubectl delete -f statefulset.yml
```
