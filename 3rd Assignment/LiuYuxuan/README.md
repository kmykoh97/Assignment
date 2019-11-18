# HW3
Toy scheduler

## 环境
```
kubernetes v1.16
go v1.13
```

## 使用方法
* 打开master proxy以获取各结点信息
```
$ kubectl proxy
Starting to serve on 127.0.0.1:8001
```

* 随便创建某个等待scheduler的deployment，这里用nginx（见nginx.yaml）
```
$ kubectl create -f nginx.yaml
deployment "nginx" created
```

* 此时的pods正处于pending状态

```
$ kubectl get pods
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   0/3     3            0           17m
```

* 编译并且运行scheduler
```
$ go build -a --ldflags '-extldflags "-static"' -tags netgo -installsuffix netgo .
$ ./scheduler
2019/11/18 14:46:49 Starting custom scheduler...
2019/11/18 14:46:51 Successfully assigned nginx-76b9cb7cf-swg5v to liu-vostro-5568
```
* 再次查看pods状态
```
$ kubectl get pods
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   3/3     3            3           18m
```

* 根据后文的priority策略，可以得到以下合理结果
```
$ sudo microk8s.kubectl get nodes
NAME               STATUS   ROLES    AGE   VERSION
ip-172-31-34-73    Ready    <none>   22h   v1.16.2
ip-172-31-44-103   Ready    <none>   23h   v1.16.2
$ sudo microk8s.kubectl describe pods | grep Node:
Node:         ip-172-31-34-73/172.31.34.73
Node:         ip-172-31-34-73/172.31.34.73
Node:         ip-172-31-34-73/172.31.34.73
```
* 将nginx.yaml中的replicas改为5，继续分配
发现资源耗尽，从log中发现，将持续分配ip-172-31-33-103/172.31.33.103
（master机器卡死打不开了，所以没有数据复制）
## 调度器介绍
* 本调度器使用两种方式确定尚未调度的任务
    * monitorUnscheduledPods： 等待两秒(以防与默认的scheduler争夺)查看```/api/v1/watch/pods```,如果有```ADDED```表明有新添加的尚未调度的Pods， 交给自定义的scheduler处理
    * reconcileUnscheduledPods： 每30秒执行一次，查看是否有由于意外情况尚未调度的实例，而且该实例的schedulerName为SE418（正如在nginx.yaml中确定的那样），如果是则交给自定义的scheduler处理。
* predicate
    * fit：利用fit函数进行predicate
    * fit函数会查看```/api/v1/pods/```与```/api/v1/nodes```,从而计算出每个nodes剩余的内存与CPU
    * fit函数返回待满足要求的nodes（即待调度的pod所需要的内存与CPU均小于该node剩余资源）
* priority
    * bestPrice：利用bestPrice函数进行评分
    * 这是一个toy scheduler，而且由于我只有两个node
    * 因此评分的准则是，哪个node name对应ascii序列和越小， 就选哪个。
    * 因此只要CPU与内存足够大，就会一直选同一个node