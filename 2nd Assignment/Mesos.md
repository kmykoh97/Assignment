# Mesos: A platform for fine - grained resource sharing in the data center

## Introduction

Mesos, a thin resource sharing layer that enables ﬁne-grained sharing across diverse cluster computing frameworks, by giving frameworks a common interface for accessing cluster resources.

## Problem to solve

目前有很多集群计算框架，但没有对于任何应用都是最佳的框架，因此需要在一个集群上运行多个计算框架，于是就需要一个框架的调度器来管理对不同框架资源的分配，而且还要考虑到对新开发的计算框架的支持。

## Features of development

Mesos使用了去中心化的模式，将调度的控制委托给集群计算框架，保持了自己的简洁，也不需要对计算框架的调度功能进行重构，Mesos围绕着两个设计元素：一个在task级别的细粒度共享模型和一个resource offers分布式调度机制。

它的两个关键功能是资源分配和资源隔离。Mesos将资源分配的决定权委派给可插拔的分配模块，集群框架有对不满足自己要求的资源拒绝的机制。由于大部分任务都是短暂的，Mesos仅在任务结束时重新分配资源，对于一个充满长期任务的集群，Mesos也会撤回任务。Mesos的隔离机制是使用操作系统的容器技术实现的。比如Linux Container和Solaris Projects.

![a](https://github.com/HokhyTann/Assignment/tree/master/mesosimage/1.png)

Figure 2显示了Mesos的主要部件。

Mesos包括一个master进程，它管理在每个集群节点上运行的slave进程。而集群框架在这些slave节点上运行任务。slave进程向master进程报告它空闲的资源，master进程决定给每个进程提供多少资源，集群框架的调度器选择使用哪一种被调度的资源。当一个集群框架接受了被提供的资源，集群框架会给Mesos传递一个它想要运行在这些资源之上的任务的描述。

Mesos的master进程设计为soft state,这样一个新的master进程可以从slave节点上和集群框架调度器上的信息中完整地重建它的内部状态。并且，为了处理调度器错误，Mesos允许一个集群框架注册多个调度器，这样，当一个调度器失败时，另一个被Mesos的master进程通知替代失败的调度器

## Benefits

Mesos能够获得较高的利用率，对工作负载的改变相应快速，并且满足了对多个集群框架保持可拓展性和鲁棒性的需要，能够对集群框架做最少的修改。使用Mesos，已经存在的集群框架能够有效地共享资源，Mesos还能够支持开发专业的集群框架来提供主要的性能收益，比如Spark，并且Mesos的简单的设计允许系统容错，可以扩展到50000个节点。同时Mesos还有着轻量级的优点。

通过在Amazon Elastic Compute Cloud（EC2）上一系列的实验来评测Mesos的结果表明Mesos比静态分区得到了更高的使用率，并且完成比静态分区共享集群快，并且可能由于其他集群框架的需求的差异而变得更快。

![b](https://github.com/HokhyTann/Assignment/tree/master/mesosimage/2.png)

Figure 5比较了在共享和静态分配集群中，每个集群框架在一段时间内资源分配情况。阴影部分表示静态分配集群中的资源分配，实线区域表示在Mesos上共享的资源。我们看到当整体需求允许的情况下，细粒度的集群框架（Hadoop和Spark）利用Mesos扩展到了超过集群的1/4，结果是在Mesos上更快地提交了作业。


## Limitations of Distributed Scheduling

碎片化：当集群充满了少量资源请求的任务时，一个有大量资源请求的集群框架将会starve，当一个小的任务结束，释放的资源无法满足大任务框架的请求，调度就会持续地在小任务之间切换。

相互依赖的集群框架约束：Mesos无法处理当集群框架之间有着很深的依赖关系的情况（比如，来自两个集群框架之间的任务不能共存），虽然这样的场景在实践中很罕见。

对长期任务的支持教差：Mesos向集群建议使用短期任务，还会对占用时间过长的任务进行撤回，这些措施都对长期任务不太友好。在处理大量长期任务的框架时，Mesos的表现可能不会太好。

## Reference

* https://people.eecs.berkeley.edu/~alig/papers/mesos.pdf
* https://blog.csdn.net/u011364612/article/details/53613511


by:谭湘楠

