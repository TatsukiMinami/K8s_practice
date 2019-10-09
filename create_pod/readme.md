# やったこと
## クラスタの作成
```
 ~ minikube start -p k8s_hand_on
😄  [k8s_hand_on] minikube v1.4.0 on Darwin 10.14.6
🔥  Creating virtualbox VM (CPUs=2, Memory=4096MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.16.0 on Docker 18.09.9 ...
🚜  Pulling images ...
🚀  Launching Kubernetes ...
⌛  Waiting for: apiserver proxy etcd scheduler controller dns
🏄  Done! kubectl is now configured to use "k8s_hand_on"
```
## name spaceの作成
```
 ~ kubectl create namespace app
namespace "app" created
➜  ~ kubectl get namespace
NAME              STATUS    AGE
app               Active    11s
default           Active    6m
kube-node-lease   Active    6m
kube-public       Active    6m
kube-system       Active    6m
```

## Podの作成
```
 kubectl create -f Sites/K8s_practice/create_pod/nginx.yml -n app
pod/nginx created

 kubectl get pod -n app
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          46s
```
## wget して稼働確認

```
kubectl -n app exec -it nginx -- sh -c "wget localhost"
Connecting to localhost (127.0.0.1:80)
index.html           100% |********************************|   612  0:00:00 ETA
```

## Podの削除
```
kubectl delete pod nginx -n app
pod "nginx" deleted

kubectl get pod -n app
No resources found.
```

## ハマったところ
```
 kubectl create -f Sites/K8s_practice/create_pod/nginx.yml -n app
error: SchemaError(io.k8s.api.core.v1.NodeSelectorRequirement): invalid object doesn't have additional properties
```
どうやらシンボリックリンクが切れてる様子なので
```
rm /usr/local/bin/kubectl
➜  ~ brew link --overwrite kubernetes-cli
Linking /usr/local/Cellar/kubernetes-cli/1.15.3... 229 symlinks created
```
貼り直す
参考URL:https://stackoverflow.com/questions/55417410/kubernetes-create-deployment-unexpected-schemaerror

### wgetコマンドがない
```
kubectl -n app exec -it nginx -- sh -c "wget localhost"
sh: 1: wget: not found
command terminated with exit code 127
```
- 解決策
imageを`nginx`から`nginx:alpine`に変更
