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

* Start minikube with hyperkit:
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
      
* Create namespace `development`:
```bash
kubectl create ns development
```
      
* Create a private key for your user. In this example, we will name the file `jainish.key`:     
```bash
openssl genrsa -out jainish.key 2048
```      

* Create a certificate sign request `jainish.csr` using the private key you just created. 
  Make sure you specify your username and group in the -subj section (CN is for the username and O for the group). 
  We will use `jainish` as the name and `developer` as the group:
```bash
openssl req -new -key jainish.key -out jainish.csr -subj "/CN=jainish/O=developer"
```  

* Locate your Kubernetes cluster certificate authority (CA).In the case of Minikube, it would be `~/.minikube/`. 
  Check that the files `ca.crt` and `ca.key` exist in the location.
```bash
ls -alt ~/.minikube/
```

* Generate the final certificate `jainish.crt` by approving the certificate sign request `jainish.csr`.  
  In this example, the certificate will be valid for 500 days:
```bash
openssl x509 -req -in jainish.csr -CA /Users/jainish.shah/.minikube/ca.crt -CAkey /Users/jainish.shah/.minikube/ca.key -CAcreateserial -out jainish.crt -days 500
```

* Save both `jainish.crt` and `jainish.key` in a safe location.
```bash
mv jainish.crt /Users/jainish.shah/.minikube/
mv jainish.key /Users/jainish.shah/.minikube/
```

* Add a new context with the new credentials for your Kubernetes cluster:
```bash
kubectl config set-credentials jainish --client-certificate=/Users/jainish.shah/.minikube/jainish.crt --client-key=/Users/jainish.shah/.minikube/jainish.key
kubectl config set-context jainish-context --cluster=minikube --namespace=development --user=jainish
```

* Test `jainish-context`:
```bash
kubectl --context=jainish-context get pods
```
Note: You should get an access denied error.

* Create Role `developer`:
```bash
kubectl apply -f role.yml
```

* Create RoleBinding `developer-binding`:
```bash
kubectl apply -f rolebinding.yml 
```

* Create clusterRole, say `secret-reader`:
```bash
kubectl apply -f clusterrole.yml
```

* Create clusterRoleBinding, say `secret-reader-binding`:
```bash
kubectl apply -f clusterrolebinding.yml
```

* Set `jainish-context` as current context:
```bash
kubectl config use-context jainish-context
```

* Let's check permission by running following commands::
```bash
kubectl auth can-i get pods --all-namespaces
kubectl auth can-i get secrets --all-namespaces
kubectl auth can-i get secrets
kubectl auth can-i create pods
kubectl auth can-i delete pods
```
