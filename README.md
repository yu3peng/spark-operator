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
kubectl exec -it my-spark-cluster-w-fmw6t bash
```

进入容器后
```bash
bash-4.2$ /opt/spark-distro/spark-2.4.5-bin-hadoop2.7/bin/spark-shell
20/06/10 07:48:57 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Spark Context Web UI is available at //proxy/local-1591775384218
Spark context available as 'sc' (master = local[*], app id = local-1591775384218).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.4.5
      /_/

Using Scala version 2.11.12 (OpenJDK 64-Bit Server VM, Java 1.8.0_242)
Type in expressions to have them evaluated.
Type :help for more information.

scala>  val textFile = spark.read.textFile("/opt/spark-distro/spark-2.4.5-bin-hadoop2.7/README.md")
textFile: org.apache.spark.sql.Dataset[String] = [value: string]

scala> textFile.count()
res0: Long = 104
```

上面这两行把当前路径的 README.md 文件读入内存，每行被读成一个String，多个 String 组成Spark的 Dataset 对象，Dataset 是一种Spark的概念，现在假装它是个类似数组的东西，它包含了 104 个String

```bash
scala> textFile.first()
res1: String = # Apache Spark
```

继续，在Spark中查看我们读到的这个 Dataset 对象的第一个元素,如上：

下面来统计一下 README.md 中有多少行带有 Spark

```bash
scala> textFile.filter(line => line.contains("Spark")).count()
res2: Long = 19
```

你可能会觉得上面的这些功能用其它的代码同样可以实现，是的，如果光是处理一个100行的文件，是没有必要使用Spark的，但当你需要处理一个几十上百G甚至上T的文件时，上面的代码无需任何修改Spark能以同样的方式运行，无需去顾虑内存、文件IO、分布式通信等各种细节，并且效率非常高，这是用普通的代码实现不了的。

## 4. 删除集群可以使用以下命令
```bash
kubectl delete sparkcluster my-spark-cluster
```

