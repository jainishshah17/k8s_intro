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

* Generate base64 values of credentials
```bash
echo -n 'admin' | base64
YWRtaW4=
echo -n 'password' | base64
cGFzc3dvcmQ=
echo -n 'test_db' | base64
dGVzdF9kYg==
```
*Note:* Update `configmap-literal.yml` with base64 values of your credentials

* Create Generic secret 
```bash
kubectl apply -f configmap-literal.yml
kubectl apply -f configmap-file.yml
```

#### Docker registry secret
In case you built your own Artifactory image and pushed it to your private registry as suggested above, you might need to define a docker-registry secret to be used by Kubernetes to pull images
```bash
$ export YOUR_DOCKER_REGISTRY=docker.io
$ export USER=jainishshah17
$ export PASSWORD=API_KEY
$ kubectl create secret docker-registry docker-reg-secret --docker-server=${YOUR_DOCKER_REGISTRY} --docker-username=${USER} --docker-password=${PASSWORD} --docker-email=jainishshah@yahoo.com
```

#### SSL secret
Create the SSL secret that can be used by the Nginx pod  
**NOTE:** These are self signed key and certificate for demo use only!
```bash
$ kubectl create secret tls art-tls --cert=./ssl/demo.pem --key=./ssl/demo.key
```

* Check secret 
```bash
kubectl get secret
```

* Check describe secret
```bash
kubectl describe secret db-cred-secret
kubectl describe secret db-conf-secret
```

* Create Postgresql deployment 
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
# To print value of file from secret
cat /etc/foo/db.properties
# To print value of variable from secret
echo $POSTGRES_DB
echo $POSTGRES_USER
echo $POSTGRES_PASSWORD
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

* Delete secret 
```bash
kubectl delete -f secret-literal.yml
kubectl delete -f secret-file.yml
```
