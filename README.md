# spark-operator

## 1. 选择 Kubernetes 集群
如果没有现成的 Kubernetes 集群，可以选择网上免费的 Kubernetes 集群资源：

### 1.1 在 kata 中利用 Ubuntu 安装指定版本的 Kubernetes 集群
具体方式可以见：[ubuntu-Kubernetes](https://github.com/yu3peng/ubuntu-Kubernetes)

### 1.2 在 kata 中直接使用 Kubernetes Playgroud
登录 [Kubernetes Playgroud](https://katacoda.com/courses/kubernetes/playground) 使用，本实践采用此种方式进行。

## 2. 创建 operator 以及集群

```bash
git clone 
kubectl apply -f manifest/operator.yaml
kubectl apply -f examples/cluster.yaml
```

删除集群可以使用以下命令
```bash
kubectl delete sparkcluster my-spark-cluster
```

