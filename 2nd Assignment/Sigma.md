Name: HOKHY TANN

Student Number:
516030990002

--------------------

# Technical Report : Sigma
0. Background
1. Introduction of Sigma
1. Sigma from User's Perspective
1. Summary
1. references
## 0. Background
> Google’s Borg system is a cluster manager that runs hundreds
of thousands of  jobs, from many thousands of different
applications, across a number of clusters each with up to
tens of thousands of machines.
It achieves high utilization by combining admission control,
efficient task-packing, over-commitment, and machine
sharing with process-level performance isolation. It supports
high-availability applications with runtime features that minimize
fault-recovery time, and scheduling policies that reduce
the probability of correlated failures. Borg simplifies
life for its users by offering a declarative job specification
language, name service integration, real-time job monitoring,
and tools to analyze and simulate system behavior.
We present a summary of the Borg system architecture
and features, important design decisions, a quantitative analysis
of some of its policy decisions, and a qualitative examination
of lessons learned from a decade of operational
experience with it.

The cluster management system internally called Borg admits,
schedules, starts, restarts, and monitors the full range
of applications that Google runs. Borg provides three main benefits: it (1) hides the details
of resource management and failure handling so its users can
focus on application development instead; (2) operates with
very high reliability and availability, and supports applications
that do the same; and (3) lets us run workloads across
tens of thousands of machines effectively. Google introduced a service called Sigma in order to accomplish these features....



## 1. Introduction of Sigma
>A service called Sigma provides a web-based user interface(UI) through which a user can examine the state of all
their jobs, a particular cell, or drill down to individual jobs
and tasks to examine their resource behavior, detailed logs,
execution history, and eventual fate. Our applications generate
voluminous logs; these are automatically rotated to avoid
running out of disk space, and preserved for a while after the
task’s exit to assist with debugging. If a job is not running
Borg provides a “why pending?” annotation, together with
guidance on how to modify the job’s resource requests to
better fit the cell. We publish guidelines for “conforming”
resource shapes that are likely to schedule easily.
Sigma helps users understand and debug Borg's behavior and manage their jobs, and help google's SRE ( Website Reliability Engineers ) each manage more than tens of thousands of machines.


## 2. Sigma from User's Perspective
Users can use a web interface called Sigma to check all their job status, a special cell, or a resource usage, detailed log, operational history, and eventual fate of a task that goes deep into a job. Google's application generates a lot of logs, will be automatically scrolled to avoid stuffing the hard disk, will be in a task after the end of a short period of time to use for debugging. If a job is not running up, Borg will provide a reason why the suspension of the explanation, to guide users how to modify the job resource needs to meet the current situation of the cell. We publish the use of resources policy, in accordance with this policy to be easy to be dispatched.

## 3. Summary
Almost all of the Google cluster load has moved to Borg over the past decade. 
**Sigma helps Borg to monitor detailed infomation of all job, allows SRE to be able to manage application easily.**

## 4. References
1. [Large-scale cluster management at Google with Borg](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/43438.pdf)_

2. [USE BORG FOR LARGE-SCALE CLUSTER MANAGEMENT ON GOOGLE [FULL TEXT PDF]](http://www.dockermall.com/use-borg-for-large-scale-cluster-management-on-google-full-text-pdf/)_