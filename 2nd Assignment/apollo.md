# Apollo: Scalable and Coordinated Scheduling for Cloud-Scale Computing


## Brief Introduction

>Apollo is a highly scalable and coordinated scheduling framework, which has been deployed on production clusters at Microsoft to schedule thousands of computations with millions of tasks efficiently and effectively on tens of thousands of machines daily.


## Abstract
This paper presents the Apollo scheduling framework, which has been fully deployed to schedule jobs in cloud-scale production clusters at Microsoft, serving a variety of on-line services. Scheduling billions of tasks daily efficiently and effectively, Apollo addresses the schedul-ing challenges in large-scale clusters with the following technical contributions.
- balance scalability and scheduling quality
- achieve high-quality scheduling decisions
- supply individual schedulers with cluster infor-mation
- cope with unexpected cluster dynamics, subopti-mal estimations, and other abnormal runtime behav-iors, which are facts of life in large-scale clusters
- drive high cluster utilization while maintaining low job latencies
- ensure no service disruption or performance re-gression when we roll out Apollo to replace a previ-ous scheduler deployed in production


## Scheduling at Production Scale
Apollo serves as the underlying scheduling framework for Microsoft’s distributed computation platform, which supports large-scale data analysis for a variety of business needs.

![图片1.png](https://i.loli.net/2019/10/27/9n428Xc1KEbrvUV.png)

The picture above shows a sample execution graph in SCOPE, greatly simplified from an important production job that collects user click information and derives insights for advertisement effectiveness. Conceptually, the job per-forms a join between an unstructured user log and a structured input that is pre-partitioned by the join key. The plan first partitions the unstructured input using the partitioning scheme from the other input: stages S1 and S2 respectively partition the data and aggregate each par-tition. A partitioned join is then performed in stage S4. The DOP is set to 312 for S1 based on the input data vol-ume, set to 10 for S5, and set to 150 for S2, S3, and S4.

![图片2.png](https://i.loli.net/2019/10/27/UsJSprzvXgfh1id.png)

---
## The Apollo Framework

To support the scale and scheduling rate required for the production workload, Apollo adopts a distributed and coordinated architecture, where the scheduling of each job is performed independently and incorporates aggre-gated global cluster load information.

The picture below provides an overview of Apollo’s architecture.

![图片3.png](https://i.loli.net/2019/10/27/IFTWXvxGjPRmUZE.png)

 >A Job Manager (JM), also called a scheduler, is assigned to manage the life cycle of each job. The global cluster load information used by each JM is provided through the cooperation of two additional entities in the Apollo framework: a Resource Monitor (RM) for each cluster and a Process Node (PN) on each server. A PN pro-cess running on each server is responsible for managing the local resources on that server and performing local scheduling, while the RM aggregates load information from PNs across the cluster continuously, providing a global view of the cluster status for each JM to make informed scheduling decisions.


Apollo thereby adopts an estimation-based approach to making task scheduling decisions. Specifically, Apollo considers the wait-time matrices, aggregated by the RM, together with the individual characteristics of tasks to be sched-uled, such as the location of inputs.

Apollo adopts a variant of the stable match-ing algorithm to match tasks with servers. For each task in a batch, Apollo finds the server with the earliest estimated completion time as a proposal for that task. A server accepts a proposal from a task if that is the only proposal assigned. A conflict arises when more than one task proposes to the same server. In this case, the server picks the task whose completion time saving is the great-est if it is assigned to the server. The tasks not picked withdraw their proposals and enter the next iteration that tries to match the remaining tasks and servers. The algo-rithm iterates until all tasks have been assigned, or until it reaches the maximum number of iterations.
 
Shown in the picture below.
![图片4.png](https://i.loli.net/2019/10/27/MYjtNO23sUmHWl4.png)

The stable matcher matches Task2 to C and Task3 to B, which effectively leverages locality and re-sults in better job performance.

## Evaluation

To evaluate Apollo thoroughly, a combination of the following methods is used:
- analyze and report various system metrics on large-scale pro-duction clusters where Apollo has been deployed; further, compare the behavior before and after enabling Apollo.
-  perform in-depth studies on representa-tive production jobs to highlight our observations at per-job level
- use trace-driven simulations on certain specific points in the Apollo design to compare with al-ternatives

### Apollo at Scale
The picutre shows the peak scheduling rates for each hour over the past 6 months, highlighting that Apollo can constantly provide a scheduling rate of above 10,000, reaching up to 20,000 per second in a single cluster.
![KscCP1.png](https://s2.ax1x.com/2019/10/27/KscCP1.png)

Then drill down into a period of two weeks and re-port various aspects of Apollo, without diluting data over a long period of time.(a) shows the number of concurrently running jobs and their tasks in the cluster while (b) shows server CPU utilization in the same period, both sampled at every 10 seconds. Apollo is able to run 750 concurrent complex jobs (140,000 con-current regular tasks) and achieve over 90% CPU uti-lization when the demand is high during the weekdays, reaching closely the capacity of the cluster.

![KscnIA.png](https://s2.ax1x.com/2019/10/27/KscnIA.png)

The recurring nature of the workload produced a strong cor-relation in CPU time between the workloads in the two time frames, as shown in (a). (b) shows the CDF of the speedups for all recurring jobs and it indicates that about 80% of recurring jobs receive var-ious degrees of performance improvements (up to 3x in speedup) with Apollo. To understand the reason for the differences, we measured the average task queuing time on each server for every window of 10 minutes. (c) shows the standard deviation of the average task queue time across servers, comparing Apollo with the baseline scheduler, which indicates clearly that Apollo achieves much more balanced task queues across servers.

![KsccdJ.png](https://s2.ax1x.com/2019/10/27/KsccdJ.png)

The picture shows job performance using Apollo and the baseline scheduler, respectively, and compares them with the oracle scheduler using runtime traces. On aver-age, the job latency improved around 22% with Apollo over the baseline scheduler, and Apollo performed within 4.5% of the oracle scheduler. On some days, Apollo is even better than the oracle scheduler with the capac-ity constraint because the job is able to get some extra speedup from opportunistic scheduling, allowing the job to get more capacity than the capacity constraint used by the oracle scheduler.

![Ksgy1P.png](https://s2.ax1x.com/2019/10/27/Ksgy1P.png)

Then measure the accuracy of the estimated task wait time, as a result of applying task resource estima-tion to the wait-time matrix. Over 95% tasks have a wait time estimation error of less than 1 second. Then mea-sure the CDF of the estimation error for task CPU time, as shown in Figure 10. For 75% of tasks, the CPU time predicted when the task is scheduled is within 25% of the actual CPU consumption.

![Ksg4ts.png](https://s2.ax1x.com/2019/10/27/Ksg4ts.png)

The picture below shows the distributions of task queuing time. With estimation enabled, Apollo achieves much more balanced scheduling across servers, which in turn leads to shorter task queuing latency.

![KsgT10.png](https://s2.ax1x.com/2019/10/27/KsgT10.png)

## Conclusion

This paper present Apollo, a scalable and coor-dinated scheduling framework for cloud-scale comput-ing. Apollo adopts a distributed and loosely coordinated scheduling architecture that scales well without sacrific-ing scheduling quality. Each Apollo scheduler consid-ers various factors holistically and performs estimation-based scheduling to minimize task completion time. By maintaining a local task queue on each server, Apollo enables each scheduler to reason about future resource availability and implement a deferred correction mecha-nism to effectively adjust suboptimal decisions dynami-cally. To leverage idle system resources gracefully, op-portunistic scheduling is used to maximize the overall system utilization. Apollo has been deployed on produc-tion clusters at Microsoft: it has been shown to achieve high utilization and low latency, while coping well with the dynamics in diverse workloads and large clusters.