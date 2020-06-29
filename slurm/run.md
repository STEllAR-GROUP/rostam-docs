---
title: Running Jobs
description: 
published: true
date: 2020-06-29T02:58:37.742Z
tags: 
editor: markdown
---

## SLURM Workload Manager

Rostam uses [SLURM](https://slurm.schedmd.com/) to allocate resources and launch jobs.

>SLURM has an extensive [documentaion](https://slurm.schedmd.com/quickstart.html). We will cover just some basics here.{.is-success}

### Terminology

In the Slurm terminlogy **nodes** are the compute resource. The node are grouped into logical (possibly overlapping) sets called **partitions** are groups of nodes into a logical (possibly overlapping) sets. The partitions can be considered as job **queues**, each of which could have its own settings such as the job size limit, the job time limit and the users permitted to use the partition, etc. **jobs** or **allocations** are resources assigned to a user for a specified amount of time.

>On Rostam the unit of allocation is a node, you can not allocate half a node.{.is-info}

### Basic Slurm Commands

#### sinfo
Use `sinfo` to see what partitions exist on the system, what nodes they include, and general system state.

```bash
$ sinfo 
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST 
medusa*      up 3-00:00:00     16   idle medusa[00-15] 
cuda         up 3-00:00:00      1  alloc reno 
cuda         up 3-00:00:00      4   idle bahram,carson,diablo,geev 
QxV100       up 3-00:00:00      1   idle diablo 
v100         up 3-00:00:00      1   idle geev 
k80          up 3-00:00:00      1   idle bahram 
k40          up 3-00:00:00      1  alloc reno 
fury         up 3-00:00:00      1  alloc reno 
```

#### squeue
To determine what jobs exist on the system use the `squeue` command. 

```bash
$ squeue 
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
 7504      sc13     bash  alireza  R       7:35      1 reno
 7505      sc13 hostname  alireza PD       0:00      2 (Resources)
```

The `ST` field is job state. A jobs is in a running state (`R` indicates *Running*) and a job in pending state (`PD` indicates *Pending*). The TIME field shows how long the jobs have been running (the format is days-hours:minutes:seconds). The NODELIST(REASON) field indicates where the job is running or the reason it is still pending. Typical reasons for pending jobs are Resources (waiting for resources to become available) and Priority (queued behind a higher priority job).

>{.is-warning}

