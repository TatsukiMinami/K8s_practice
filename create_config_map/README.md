## 前回作ったクラスタの起動
```
minikube start -p k8s_hand_on
😄  [k8s_hand_on] minikube v1.4.0 on Darwin 10.14.6
🔄  Starting existing virtualbox VM for "k8s_hand_on" ...
⌛  Waiting for the host to be provisioned ...
🐳  Preparing Kubernetes v1.16.0 on Docker 18.09.9 ...
🔄  Relaunching Kubernetes using kubeadm ...
⌛  Waiting for: apiserver proxy etcd scheduler controller dns
🏄  Done! kubectl is now configured to use "k8s_hand_on"
```
## configMapの作成
```
kubectl apply -f nginx-config.yml
configmap/nginx-config created

kubectl -n app get configmap nginx-config
NAME           DATA   AGE
nginx-config   1      80s
```

## Podの作成
```
kubectl apply -f nginx.yml
pod/nginx created
```


 ## curlを入れる
 ```
 kubectl exec -n app -it nginx ash
 / #
 
 apk add --update curl
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/community/x86_64/APKINDEX.tar.gz
(1/4) Installing ca-certificates (20190108-r0)
(2/4) Installing nghttp2-libs (1.39.2-r0)
(3/4) Installing libcurl (7.66.0-r0)
(4/4) Installing curl (7.66.0-r0)
Executing busybox-1.30.1-r2.trigger
Executing ca-certificates-20190108-r0.trigger
OK: 30 MiB in 41 packages
```

## localhostに投げてみる
```
 curl -I localhost
HTTP/1.1 302 Moved Temporarily
Server: nginx/1.17.4
Date: Thu, 17 Oct 2019 04:04:23 GMT
Content-Type: text/html
Content-Length: 145
Connection: keep-alive
Location: https://google.com
```

## podとconfig_mapの削除
```
kubectl -n app delete pod nginx
pod "nginx" deleted

 kubectl -n app delete configMap nginx-config        configmap "nginx-config" deleted
 ```

 ##  ハマったところ
 - 特になし
