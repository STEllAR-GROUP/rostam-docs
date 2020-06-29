---
title: Running Jobs
description: 
published: true
date: 2020-06-29T17:42:42.695Z
tags: 
editor: markdown
---

## SLURM Workload Manager

Rostam uses [SLURM](https://slurm.schedmd.com/) to allocate resources and launch jobs.

>Slurm has an extensive [documentaion](https://slurm.schedmd.com/quickstart.html). We will cover just some basics here.{.is-success}

### Terminology

In the Slurm terminlogy **nodes** are the compute resource. The node are grouped into logical (possibly overlapping) sets called **partitions** are groups of nodes into a logical (possibly overlapping) sets. The partitions can be considered as job **queues**, each of which could have its own settings such as the job size limit, the job time limit and the users permitted to use the partition, etc. **jobs** or **allocations** are resources assigned to a user for a specified amount of time, and **job steps** are sets of (possibly parallel) tasks within a job.

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

#### scontrol

The `scontrol` command can be used to report more detailed information about nodes, partitions, jobs, job steps, and configuration.

```bash
$ scontrol show partition
PartitionName=medusa
   AllowGroups=ALL AllowAccounts=ALL AllowQos=ALL
   AllocNodes=ALL Default=YES QoS=N/A
   DefaultTime=04:00:00 DisableRootJobs=NO ExclusiveUser=NO GraceTime=0 Hidden=NO
   MaxNodes=UNLIMITED MaxTime=3-00:00:00 MinNodes=0 LLN=NO MaxCPUsPerNode=UNLIMITED
   Nodes=medusa[00-15]
   PriorityJobFactor=1 PriorityTier=1 RootOnly=NO ReqResv=NO OverSubscribe=NO
   OverTimeLimit=NONE PreemptMode=OFF
   State=UP TotalCPUs=640 TotalNodes=16 SelectTypeParameters=NONE
   JobDefaults=(null)
   DefMemPerNode=UNLIMITED MaxMemPerNode=UNLIMITED

PartitionName=cuda
   AllowGroups=ALL AllowAccounts=ALL AllowQos=ALL
   AllocNodes=ALL Default=NO QoS=N/A
   DefaultTime=04:00:00 DisableRootJobs=NO ExclusiveUser=NO GraceTime=0 Hidden=NO
   MaxNodes=UNLIMITED MaxTime=3-00:00:00 MinNodes=0 LLN=NO MaxCPUsPerNode=UNLIMITED
   Nodes=geev,bahram,reno,diablo,carson
   PriorityJobFactor=1 PriorityTier=1 RootOnly=NO ReqResv=NO OverSubscribe=NO
   OverTimeLimit=NONE PreemptMode=OFF
   State=UP TotalCPUs=120 TotalNodes=5 SelectTypeParameters=NONE
   JobDefaults=(null)
   DefMemPerNode=UNLIMITED MaxMemPerNode=UNLIMITED

...
```

```bash
$ scontrol show node bahram
NodeName=bahram Arch=x86_64 CoresPerSocket=10 
   CPUAlloc=0 CPUTot=20 CPULoad=0.00
   AvailableFeatures=(null)
   ActiveFeatures=(null)
   Gres=(null)
   NodeAddr=bahram NodeHostName=bahram Version=20.02.3
   OS=Linux 4.18.0-193.6.3.el8_2.x86_64 #1 SMP Wed Jun 10 11:09:32 UTC 2020 
   RealMemory=1 AllocMem=0 FreeMem=125572 Sockets=2 Boards=1
   State=IDLE ThreadsPerCore=1 TmpDisk=0 Weight=1 Owner=N/A MCS_label=N/A
   Partitions=cuda,k80,sc15 
   BootTime=2020-06-28T12:29:52 SlurmdStartTime=2020-06-28T21:50:31
   CfgTRES=cpu=20,mem=1M,billing=20
   AllocTRES=
   CapWatts=n/a
   CurrentWatts=0 AveWatts=0
   ExtSensorsJoules=n/s ExtSensorsWatts=0 ExtSensorsTemp=n/s
```

```bash
$ scontrol show job
JobId=7517 JobName=bash
   UserId=alireza(868800001) GroupId=alireza(868800001) MCS_label=N/A
   Priority=4294901739 Nice=0 Account=stellar QOS=normal
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=0 Reboot=0 ExitCode=0:0
   RunTime=00:12:15 TimeLimit=04:00:00 TimeMin=N/A
   SubmitTime=2020-06-29T11:05:22 EligibleTime=2020-06-29T11:05:22
   AccrueTime=Unknown
   StartTime=2020-06-29T11:05:22 EndTime=2020-06-29T15:05:22 Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2020-06-29T11:05:22
   Partition=sc13 AllocNode:Sid=caravaggio:279945
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=reno
   BatchHost=reno
   NumNodes=1 NumCPUs=20 NumTasks=1 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
   TRES=cpu=20,node=1,billing=20
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=1 MinMemoryNode=0 MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=NO Contiguous=0 Licenses=(null) Network=(null)
   Command=/bin/bash
   WorkDir=/home/alireza
   Power=
   MailUser=(null) MailType=NONE

...
```

#### srun

`srun` allocates resources and launches the taks for the job in a single command `srun [options] <executable> [args]`. Some common options for srun include:

 - `-p` partition or queue 
 - `-N` number of nodes to allocate
 - `-w` explicitly specifies the nodes to allocate
 - `-n` number of proecesses to launch
 - `-t` time limit on the job allocation
 
**Example 1**: Allocating 4 nodes from default partition and run `hostname` command
```bash
rostam0 ~]$ srun -N 4 hostname
medusa03.rostam.cct.lsu.edu
medusa02.rostam.cct.lsu.edu
medusa04.rostam.cct.lsu.edu
medusa01.rostam.cct.lsu.edu
```

**Example 2**: Allocate 2 nodes from *marvin* partition and launch total of 6 prrocesses (3 each) and run `hostname -s` command.

```bash
rostam0 ~]$ srun -p marvin -N 2 -n 6 hostname -s
marvin01
marvin02
marvin01
marvin01
marvin02
marvin02
```

**Example 3**: Same as *example 2* but this time explicitly specify the nodes.

```bash
rostam0 ~]$ srun -p marvin -w "marvin[09-10]" -n 6 hostname -s
marvin10
marvin10
marvin10
marvin09
marvin09
marvin09
```

**Example 4**: Allocate a node form *medusa* partition for one hour and launch an interactive session 

```bash
rostam0 ~]$ srun -p medusa -N 1 -t 1:00:00 --pty /bin/bash -l
medusa01 ~]$ 
```

> Rostam does not let you login to any compute node unless you have a running job on that node (the node is allocated to you).{.is-warning}

#### sbatch

It makes it possible to organize a series of operation in a script to be executed. The options can be supplied as desired by using a prefix `#SBATCH` followed by the option at the beginning of the script (before any commands to be executed in the script). Options supplied on the command line would override any options specified within the script.

`myjob.sh`:

```bash
#!/bin/bash
#
#SBATCH --job-name=test_sbatch
#SBATCH --output=sbatch.out
#SBATCH --partition=medusa
#SBATCH --ntasks=4
#SBATCH --nodes=2
#SBATCH --time=10:00

# Normal shell scripts starts here. The options above would apply to all srun commands.
module load gcc
srun hostname -s
srun uname -r
```

execute:

```bash
$ sbatch myjob.sh 
Submitted batch job 7533
```

see the output:

```bash
$ cat sbatch.out 
medusa02
medusa03
medusa02
medusa03
4.18.0-193.6.3.el8_2.x86_64
4.18.0-193.6.3.el8_2.x86_64
4.18.0-193.6.3.el8_2.x86_64
4.18.0-193.6.3.el8_2.x86_64
```
> Options supplied on the command line would override any options specified within the script. {.is-info}
