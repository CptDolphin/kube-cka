alias k='kubectl'
k get pods -o=jsonpath="{.items[*]['metadata.name', 'metadata.namespace']}"
k get pods -o jsonpath='{.spec.containers[].image}' # Shows version of image without describe command
k run nginx --image nginx --restart Never --dry-run -o yaml > nginx_pod.yaml
k get po nginx -o yaml --export #get po info without cluster info
k delete po nginx --grace-period 0 --force
k set image pod/nginx nginx=nginx.1.15-alpine #live change image of pod
k exec -it nginx /bin/bash
k get po nginx -o wide
k run busybox --image busybox --restart Never -- ls #run pod and exec ls immediately
k logs busybox
k logs busybox -p #if pod crashed, logs from prev log of pod
k run busybox --image busybox --restart Never -- /bin/bash -c 'sleep 2500'
k run busybox --image nginx --restart Never -it --rm -- echo 'How are you' #create, say, delete
k get po nginx --v 7
k get pods --sort-by .metadata.name
k get pods --sort-by .metadata.creationTimestamp
