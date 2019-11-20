## helmのインストール
すでにインストール済みだったのでupgrade
```
brew upgrade helm
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 1 tap (homebrew/core).
==> Updated Formulae
reminiscence                                                                              swagger-codegen

==> Upgrading 1 outdated package:
helm 2.14.3 -> 3.0.0
==> Upgrading helm
==> Downloading https://homebrew.bintray.com/bottles/helm-3.0.0.mojave.bottle.tar.gz
==> Downloading from https://akamai.bintray.com/4b/4bdc858b036df97b65e62168506232084d056536438d755f676e2f8d51e99de6?__gda__=exp=1574266928~hmac=c9583b8937f9a2bcb44d6d1023dafc1d0c
######################################################################## 100.0%
==> Pouring helm-3.0.0.mojave.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/helm/3.0.0: 7 files, 40.6MB
Removing: /usr/local/Cellar/helm/2.14.3... (51 files, 91.6MB)
```

## minikubeの起動
```
minikube start -p k8s_hand_on

😄  [k8s_hand_on] minikube v1.5.2 on Darwin 10.14.6
👍  Upgrading from Kubernetes 1.16.0 to 1.16.2
💿  Downloading VM boot image ...
    > minikube-v1.5.1.iso.sha256: 65 B / 65 B [--------------] 100.00% ? p/s 0s
    > minikube-v1.5.1.iso: 143.76 MiB / 143.76 MiB [-] 100.00% 9.85 MiB p/s 14s
🔄  Starting existing virtualbox VM for "k8s_hand_on" ...
⌛  Waiting for the host to be provisioned ...
🐳  Preparing Kubernetes v1.16.2 on Docker '18.09.9' ...
💾  Downloading kubelet v1.16.2
💾  Downloading kubeadm v1.16.2
🚜  Pulling images ...
🔄  Relaunching Kubernetes using kubeadm ...
⌛  Waiting for: apiserver
🏄  Done! kubectl is now configured to use "k8s_hand_on"
```
## stable/mysqlの確認
```
 helm search repo stable/mysql
NAME            	CHART VERSION	APP VERSION	DESCRIPTION
stable/mysql    	1.4.0        	5.7.27     	Fast, reliable, scalable, and easy to use open-...
stable/mysqldump	2.6.0        	2.4.1      	A Helm chart to help backup MySQL databases usi...
```

## value.ymlの取得
```
 helm inspect values stable/mysql > value.yml
 ```
 ## mysqlのinstall
 ```
 helm install -name mysql -f value.yml stable/mysql --namespace app

 AME: mysql
LAST DEPLOYED: Thu Nov 21 02:02:01 2019
NAMESPACE: app
STATUS: deployed
REVISION: 1
NOTES:
MySQL can be accessed via port 3306 on the following DNS name from within your cluster:
mysql.app.svc.cluster.local

To get your root password run:

    MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace app mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

To connect to your database:

1. Run an Ubuntu pod that you can use as a client:

    kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il

2. Install the mysql client:

    $ apt-get update && apt-get install mysql-client -y

3. Connect using the mysql cli, then provide your password:
    $ mysql -h mysql -p

To connect to your database directly from outside the K8s cluster:
    MYSQL_HOST=$(kubectl get nodes --namespace app -o jsonpath='{.items[0].status.addresses[0].address}')
    MYSQL_PORT=$(kubectl get svc --namespace app mysql -o jsonpath='{.spec.ports[0].nodePort}')

    mysql -h ${MYSQL_HOST} -P${MYSQL_PORT} -u root -p${MYSQL_ROOT_PASSWORD}
 ```

 ## deploymentの確認
 ```
 kubectl -n app get deployment mysql
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
mysql   1/1     1            1           4m53s
```
## serviceの確認
```
kubectl -n app get service
NAME    TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
mysql   NodePort   10.100.222.81   <none>        3306:32000/TCP   2m44s
```
## mysql cliにログイン
```
mysql -uroot -ppassword -h $(minikube ip -p k8s_hand_on) -P32000
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 127
Server version: 5.7.14 MySQL Community Server (GPL)

Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

## mysqlの削除
```
 helm uninstall mysql --namespace  app
release "mysql" uninstalled
```

## ハマったところ
- helmのバージョンアップをしていろいろ変わってしまった（しっかり触ってどっかにまとめたい）
- minikube ip する際に`-p`オプションをつけ忘れる

