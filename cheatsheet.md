### Life all pods with name and ns using jsonpath
```
kubectl get pods -o=jsonpath="{.items[*]['metadata.name', 'metadata.namespace']}"
```

### Show version of image without describe command
```
kubectl get pods -o jsonpath='{.spec.containers[].image}' 
```

### Get template of pod that would be executed
```
kubectl run nginx --image nginx --restart Never --dry-run -o yaml > nginx_pod.yaml
```

### Get pod info without cluster info
```
kubectl get po nginx -o yaml --export 
```

### Force delete a pod
```
kubectl delete po nginx --grace-period 0 --force
```

### Change pods image live
```
kubectl set image pod/nginx nginx=nginx.1.15-alpine 
```

### Log into pods sessions
```
kubectl exec -it nginx /bin/bash
```

### Get IP address of the pod
```
kubectl get po nginx -o wide
```

### If the pod crashes shows logs from --previous pod why it did
```
kubectl logs busybox -p 
```

### Create a pod and make it execute command when it starts
```
kubectl run busybox --image busybox --restart Never -- /bin/bash -c 'sleep 2500'
```

### Create a pod, make it execute command and automatically delete
```
kubectl run busybox --image nginx --restart Never -it --rm -- echo 'How are you' 
```

### Get pod info with lvl 7 debugging
```
kubectl get po nginx --v 7
```

### Sort pods by name/timestamp
```
kubectl get pods --sort-by .metadata.name
kubectl get pods --sort-by .metadata.creationTimestamp
```

### Run pods with label
```
kubectl run nginx-dev1 --image=nginx --restart=Never --labels env=dev
kubectl run nginx-dev2 --image=nginx --restart=Never --labels env=dev
kubectl run nginx-dev3 --image=nginx --restart=Never --labels env=dev
kubectl run nginx-prod4 --image=nginx --restart=Never --labels env=prod
kubectl run nginx-prod5 --image=nginx --restart=Never --labels env=prod
```

### Get pods with label env
```
kubectl get pods -L env
```

### Get pods with label env=prod
```
kubectl get pods -l env=prod
```

### Get pods with labels env=dev or env=prod
```
kubectl get pods -l 'env in (dev,prod)' --show-labels
```

### Overwrite tag of pod
```
kubectl label pod/nginx-dev3 env=uat --overwrite
```

### Remove labels of pods
```
kubectl label pod nginx-dev{1..3} env-
kubectl label pod nginx-prod{1..2} env-
```

### Add labels to multiple pods
```
kubectl label pod nginx-dev{1..3} app=nginx
kubectl label pod nginx-prod{1..2} app=nginx
```

### Get all the nodes with labels
```
kubectl get nodes --show-labels
```

### Label the node
```
kubectl label node minikube writeHereName=nginxnode
```

---
## Secrets

### Get secret name, get its current data
```
kubectl get secrets
kubectl get secret <name> -o yaml
```

### Encrypt new data and push into secret
```
echo -n 'password' | base64
cat <<EOF> secret.yaml
apiVersion: v1
kind: Secret
metadata:
name: mysecret
type: Opaque
data:
<prev data truncated from `get secret -o yaml`>
password: em9tYmll
EOF
```
