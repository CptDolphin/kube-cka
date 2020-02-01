## Kubertnetes Cluster Architecture
### Master Node
* API Server - communication hub for all cluster components, it exposes kubernetes api
* Scheduler - Auto-Assigns app to worker node based on resource requirements
* Controller Manager - Maintains cluster, handles node failures, maintain correct number of pods etc.
* etcd - data store that stores cluster configuration

### Worker Node
* kubelet - runs and manages the containers on the node, and talks to the API Server
* kube-proxy - load balances traffic between the application components
* container-runtime - program that runs your container (docker, rkt, containerd)

#### Usefull commands:
||command||description||
|kubectl get nodes|list all the nodes in the cluster|
|kubectl get pods --all-namespaces|list pods in all namespaces|
|kubectl get pods --all-namespaces -o wide|list all the pods in the cluster in detail|
|kubectl get namespaces|show all the namespace names|
|kubectl describe pod nginx|all detail about the pod nginx|
|kubectl delete pod nginx|delete the pod nginx|

#### Create nginx pod
```
cat << EOF | kubectl create -f -
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
EOF
```

#### Official Kube Documentation
[Kubernetes Components Overview](https://kubernetes.io/docs/concepts/overview/components/)
[Api Server](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)
[Scheduler](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/)
[Controller Manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)
[etcd Datastore](https://kubernetes.io/docs/concepts/overview/components/#etcd)
[kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/)
[kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)
[Container Runtime](https://kubernetes.io/docs/concepts/overview/components/#container-runtime)
---

## Kubernetes API Primitives

#### Create deployment of nginx
```
cat <EOF > nginx.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
EOF
```

#### Usefull commands
||Command||Description||
|kubectl create -f nginx.yaml|Create deployment from YAML|
|kubectl get deployment nginx-deployment -o yaml|Get the full YAML back|
|kubectl get pods --show-labels|Show all pod labels|
|kubectl label pods <pod-name> env=prod|Applay a label to a pod|
|kubectl annotate deployment nginx-deployment mycompany.com/someannotation='doug'|Annotate a deployment|
|kubectl get pods --field-selector status.phase=Running|Use field selectors|

#### Official Kube Documentation
[Spec and Status](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/#object-spec-and-status)
[API Versioning](https://kubernetes.io/docs/concepts/overview/kubernetes-api/#api-versioning)
[Field Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/field-selectors/)
