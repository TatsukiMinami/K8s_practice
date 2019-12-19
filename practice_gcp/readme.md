## プロジェクトの設定

```
gcloud config set project k8-practice-262502
Updated property [core/project].
```

## リージョンの指定
```
 gcloud config set compute/zone us-west1-a
Updated property [compute/zone].
```
## クラスタの作成
```
 gcloud container clusters create k8spractice
WARNING: In June 2019, node auto-upgrade will be enabled by default for newly created clusters and node pools. To disable it, use the `--no-enable-autoupgrade` flag.
WARNING: Starting in 1.12, new clusters will have basic authentication disabled by default. Basic authentication can be enabled (or disabled) manually using the `--[no-]enable-basic-auth` flag.
WARNING: Starting in 1.12, new clusters will not have a client certificate issued. You can manually enable (or disable) the issuance of the client certificate using the `--[no-]issue-client-certificate` flag.
WARNING: Currently VPC-native is not the default mode during cluster creation. In the future, this will become the default mode and can be disabled using `--no-enable-ip-alias` flag. Use `--[no-]enable-ip-alias` flag to suppress this warning.
WARNING: Starting in 1.12, default node pools in new clusters will have their legacy Compute Engine instance metadata endpoints disabled by default. To create a cluster with legacy instance metadata endpoints disabled in the default node pool, run `clusters create` with the flag `--metadata disable-legacy-endpoints=true`.
WARNING: Your Pod address range (`--cluster-ipv4-cidr`) can accommodate at most 1008 node(s).
This will enable the autorepair feature for nodes. Please see https://cloud.google.com/kubernetes-engine/docs/node-auto-repair for more information on node autorepairs.
ERROR: (gcloud.container.clusters.create) ResponseError: code=403, message=Kubernetes Engine API is not enabled for this project. Please ensure it is enabled in Google Cloud Console and try again: visit https://console.cloud.google.com/apis/api/container.googleapis.com/overview?project=k8-practice-262502 to do so.
jiraffe40068noMacBook-puro:~ jiraffe40068 $ gcloud container clusters create k8spractice
WARNING: In June 2019, node auto-upgrade will be enabled by default for newly created clusters and node pools. To disable it, use the `--no-enable-autoupgrade` flag.
WARNING: Starting in 1.12, new clusters will have basic authentication disabled by default. Basic authentication can be enabled (or disabled) manually using the `--[no-]enable-basic-auth` flag.
WARNING: Starting in 1.12, new clusters will not have a client certificate issued. You can manually enable (or disable) the issuance of the client certificate using the `--[no-]issue-client-certificate` flag.
WARNING: Currently VPC-native is not the default mode during cluster creation. In the future, this will become the default mode and can be disabled using `--no-enable-ip-alias` flag. Use `--[no-]enable-ip-alias` flag to suppress this warning.
WARNING: Starting in 1.12, default node pools in new clusters will have their legacy Compute Engine instance metadata endpoints disabled by default. To create a cluster with legacy instance metadata endpoints disabled in the default node pool, run `clusters create` with the flag `--metadata disable-legacy-endpoints=true`.
WARNING: Your Pod address range (`--cluster-ipv4-cidr`) can accommodate at most 1008 node(s).
This will enable the autorepair feature for nodes. Please see https://cloud.google.com/kubernetes-engine/docs/node-auto-repair for more information on node autorepairs.
Creating cluster k8spractice in us-west1-a... Cluster is being health-checked.
..⠶
Creating cluster k8spractice in us-west1-a... Cluster is being health-checked
(master is healthy)...done.
Created [https://container.googleapis.com/v1/projects/k8-practice-262502/zones/us-west1-a/clusters/k8spractice].
To inspect the contents of your cluster, go to: https://console.cloud.google.com/kubernetes/workload_/gcloud/us-west1-a/k8spractice?project=k8-practice-262502
kubeconfig entry generated for k8spractice.
NAME         LOCATION    MASTER_VERSION  MASTER_IP        MACHINE_TYPE   NODE_VERSION    NUM_NODES  STATUS
k8spractice  us-west1-a  1.13.11-gke.14  104.196.246.137  n1-standard-1  1.13.11-gke.14  3          RUNNING
```

## 認証情報の取得
```
gcloud container clusters get-credentials k8spractice
Fetching cluster endpoint and auth data.
kubeconfig entry generated for k8spractice.
```

## deploymentの作成

```
 kubectl run hello-server --image gcr.io/google-samples/hello-app:1.0 --port 8080
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/hello-server created
```

## deploymentの公開
```
kubectl expose deployment hello-server --type "LoadBalancer"
service/hello-server exposed
```

##　確認
```
kubectl get service hello-server
NAME           TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)          AGE
hello-server   LoadBalancer   10.15.246.27   EXTERNAL_IP  8080:30224/TCP   2m18s
```

```
curl EXTERNAL_IP:8080
Hello, world!
Version: 1.0.0
```

## クリーンアップ
```
kubectl delete service hello-server
service "hello-server" deleted
```
```
 gcloud container clusters delete k8spractice
The following clusters will be deleted.
 - [k8spractice] in [us-west1-a]

Do you want to continue (Y/n)?  y

Deleting cluster k8spractice...done.
Deleted [https://container.googleapis.com/v1/projects/k8-practice-262502/zones/us-west1-a/clusters/k8spractice].
```

