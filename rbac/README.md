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

* Check if RBAC is enabled in your k8s cluster:
```bash
kubectl api-versions | grep rbac
```
Note: If RBAC is enabled you should see the API version `rbac.authorization.k8s.io/v1`
      
* Create a ServiceAccount, say 'jainish':
```bash
kubectl create serviceaccount jainish
```

* Create Role `secret-reader`:
```bash
kubectl create role secret-reader --verb=get --verb=list --verb=watch --resource=secrets
```

* Create RoleBinding `secret-reader-binding`:
```bash
kubectl create rolebinding secret-reader-binding --role=secret-reader --serviceaccount=default:jainish --namespace=default
```

* Create clusterRole, say `pod-reader`:
```bash
kubectl create clusterrole pod-reader --verb=get --verb=list --verb=watch --resource=pods
```

* Create clusterRoleBinding, say `pod-reader-binding`:
```bash
kubectl create clusterrolebinding pod-reader-binding --serviceaccount=default:jainish --clusterrole=pod-reader
```

* Let's get the token from secret of ServiceAccount we have created. We will use this token to authenticate user:
```bash
TOKEN=$(kubectl describe secrets "$(kubectl describe serviceaccount jainish | grep -i Tokens | awk '{print $2}')" | grep token: | awk '{print $2}')
```

* Now set the credentials for the user in kube config file. I am using `jainish` as username:
```bash
kubectl config set-credentials jainish --token=$TOKEN
```

* Let's Create a Context say `developer`. I am using my clustername `minikube` here:
```bash
kubectl config set-context developer --cluster=minikube --user=jainish
```

* Finally, use the context:
```bash
kubectl config use-context developer
```

* Now you can execute `kubectl get pods --all-namespaces`.

* Let's check permission by running following commands:
```bash
kubectl auth can-i get pods --all-namespaces
kubectl auth can-i get secrets --all-namespaces
kubectl auth can-i get secrets
kubectl auth can-i create pods
kubectl auth can-i delete pods
```


