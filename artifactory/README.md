### Artifactory Pro

#### Docker registry secret
In case you built your own Artifactory image and pushed it to your private registry as suggested above, you might need to define a docker-registry secret to be used by Kubernetes to pull images
```bash
$ kubectl create secret docker-registry docker-reg-secret --docker-server=${YOUR_DOCKER_REGISTRY} --docker-username=${USER} --docker-password=${PASSWORD} --docker-email=you@domain.com
```
#### SSL secret
Create the SSL secret that will be used by the Nginx pod  
**NOTE:** These are self signed key and certificate for demo use only!
```bash
$ kubectl create secret tls art-tls --cert=./files/nginx/ssl/demo.pem --key=./files/nginx/ssl/demo.key
```

#### Database (using PostgreSQL)
```bash
# PostgreSQL storage, pods and service
$ kubectl apply -f postgresql-storage.yml
$ kubectl apply -f postgresql.yml
```

#### Artifactory
```bash
# Artifactory storage, pods and service
$ kubectl apply -f artifactory-storage.yml
$ kubectl apply -f artifactory.yml
```

#### Nginx
```bash
# Nginx storage and deployment
$ kubectl apply -f nginx-storage.yml
$ kubectl apply -f nginx-deployment.yml

# Nginx service
# If running on Minikube
$ kubectl apply -f nginx-service.yml
```

Once done, you should be able to see the deployed pods and services
```bash
# Get pods and their status
$ kubectl get pods
NAME                                          READY     STATUS    RESTARTS   AGE
artifactory-k8s-deployment-1732455857-7w6gw   1/1       Running   0          31m
nginx-k8s-deployment-3171003233-q8gb2         1/1       Running   0          25m
postgresql-k8s-deployment-1240329637-25325    1/1       Running   0          33m

# Get services
# On a standard Kubernetes cluster
$ kubectl get services
NAME                     CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
artifactory              10.0.160.189   <nodes>       8081/TCP         31m
kubernetes               10.0.0.1       <none>        443/TCP          3d
nginx-k8s-service        10.0.26.194    59.156.13.6   80/TCP,443/TCP   25m
postgresql-k8s-service   10.0.172.76    <none>        5432/TCP         33m

# On Minikube
$ kubectl get services
NAME                     CLUSTER-IP   EXTERNAL-IP   PORT(S)                      AGE
artifactory              10.0.0.210   <nodes>       8081:32355/TCP               57m
kubernetes               10.0.0.1     <none>        443/TCP                      1h
nginx-k8s-service        10.0.0.113   <nodes>       80:30002/TCP,443:32600/TCP   48m
postgresql-k8s-service   10.0.0.165   <none>        5432/TCP                     1h

```
