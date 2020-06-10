# spark-operator

## 1. 选择 Kubernetes 集群
如果没有现成的 Kubernetes 集群，可以选择网上免费的 Kubernetes 集群资源：

### 1.1 在 kata 中利用 Ubuntu 安装指定版本的 Kubernetes 集群
具体方式可以见：[ubuntu-Kubernetes](https://github.com/yu3peng/ubuntu-Kubernetes)

### 1.2 在 kata 中直接使用 Kubernetes Playgroud
登录 [Kubernetes Playgroud](https://katacoda.com/courses/kubernetes/playground) 使用，本实践采用此种方式进行。

## 2. 创建 operator 以及集群

```bash
git clone https://github.com/yu3peng/spark-operator.git && cd spark-operator
kubectl apply -f manifest/operator.yaml 
```

利用以下命令观察spark-operator pod 状态
```bash
watch kubectl get pods
```

等待 spark-operator pod 状态为running 后，继续键入以下命令
```bash
kubectl apply -f examples/cluster.yaml
```

利用以下命令观察 my-spark-cluster-m pod、my-spark-cluster-w pod 状态
```bash
watch kubectl get pods
```

## 3. 运行 spark 应用
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/staging/spark/zeppelin-controller.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/staging/spark/zeppelin-service.yaml
```

检查 zeppelin 状态
```bash
kubectl get pods -l component=zeppelin
```



删除集群可以使用以下命令
```bash
kubectl delete sparkcluster my-spark-cluster
```

