---
title: Running Jobs
description: 
published: true
date: 2023-10-03
tags: 
editor: markdown
---

## SLURM Workload Manager

Rostam uses [SLURM](https://slurm.schedmd.com/) to allocate resources and launch jobs.

>Slurm has an extensive [documentaion](https://slurm.schedmd.com/quickstart.html). We will cover just some basics here.{.is-success}

### Terminology

In the Slurm terminlogy **nodes** are the compute resource. The node are grouped into logical (possibly overlapping) sets called **partitions**. The partitions can be considered as job **queues**, each of which could have its own settings such as the job size limit, the job time limit and the users permitted to use the partition, etc. **jobs** or **allocations** are resources assigned to a user for a specified amount of time, and **job steps** are sets of (possibly parallel) tasks within a job.

>On Rostam the unit of allocation is a CPU core, but there is no hard limit on the resource, your job can expand to all available cores on the allocated node.{.is-info}

>To avoid any other job running on the node that your job is running you can use `--exlucsive` option.{.is-info}

>There is hard limit on GPU resources, you need to exactly specify the number GPUs for you job.{.is-warning}

## Basic Slurm Commands

### sinfo
Use `sinfo` to see what partitions exist on the system, what nodes they include, and general system state.

```bash
$ sinfo 
PARTITION       AVAIL  TIMELIMIT  NODES  STATE NODELIST
medusa*            up 3-00:00:00     16   idle medusa[00-15]
buran              up 3-00:00:00     15   idle buran[00-03,05-15]
cuda               up 1-00:00:00      1   alloc diablo
cuda               up 1-00:00:00      6   idle bahram,geev,nasrin[0-1],toranj[0-1]
cuda-V100          up 1-00:00:00      1   alloc diablo
cuda-V100          up 1-00:00:00      2   idle bahram,geev
cuda-A100          up 3-00:00:00      4   idle nasrin[0-1],toranj[0-1]
cuda-A100-intel    up 1-00:00:00      2   idle toranj[0-1]
cuda-A100-amd      up 1-00:00:00      2   idle nasrin[0-1]
DGX-A100           up 3-00:00:00      1   idle anvil
mi100              up 1-00:00:00      1   idle kamand[0-1]
marvin             up 10-00:00:0     13   idle marvin[00-15]
```

### squeue
To determine what jobs exist on the system use the `squeue` command. 

```bash
$ squeue 
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
 7504      sc13     bash  alireza  R       7:35      1 reno
 7505      sc13 hostname  alireza PD       0:00      2 (Resources)
```

The `ST` field is job state. A jobs is in a running state (`R` indicates *Running*) and a job in pending state (`PD` indicates *Pending*). The TIME field shows how long the jobs have been running (the format is days-hours:minutes:seconds). The NODELIST(REASON) field indicates where the job is running or the reason it is still pending. Typical reasons for pending jobs are Resources (waiting for resources to become available) and Priority (queued behind a higher priority job).

### scontrol

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

### srun

`srun` allocates resources and launches the taks for the job in a single command `srun [options] <executable> [args]`. Some common options for srun include:

 - `-p` or `--partition=` partition or queue 
 - `-N` or `--nodes=` number of nodes to allocate
 - `-w` or `--nodelist=` explicitly specifies the nodes to allocate
 - `-n` or `--ntasks=` number of proecesses to launch
 - `-t` or `--time=` time limit on the job allocation
 - `--exclusive` allocate the node exclusively to the job, all the resources on the node are available to the job
 
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
rostam0 ~]$ srun -p medusa --exclusive -N 1 -t 1:00:00 --pty /bin/bash -l
medusa01 ~]$ 
```

> Rostam does not let you login to any compute node unless you have a running job on that node (the node is allocated to you).{.is-warning}

> For more onformation look at `srun` [documentaion](https://slurm.schedmd.com/srun.html){.is-success}


### sbatch

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
#SBATCH --exclusive

# Normal shell scripts starts here. The options above would apply to all srun commands.
module load gcc
srun hostname -s
srun uname -r
```

execute:

```bash
$ sbatch myjob.sh 
Submitted batch job 165019
```

see the output:

```bash
$ cat sbatch.out 
medusa00
medusa00
medusa01
medusa01
4.18.0-477.27.1.el8_8.x86_64
4.18.0-477.27.1.el8_8.x86_64
4.18.0-477.27.1.el8_8.x86_64
4.18.0-477.27.1.el8_8.x86_64
```
> Options supplied on the command line would override any options specified within the script. {.is-info}

## Running MPI Jobs

There three ways to launch an MPI applicatioin using SLURM:
1. Slurm directly launches the tasks and performs initialization of communications through the PMI2 or PMIx APIs. (Supported by most modern MPI implementations.)
1. Slurm creates a resource allocation for the job and then `mpirun` launches tasks using Slurm's infrastructure (older versions of OpenMPI).
1. Slurm creates a resource allocation for the job and then mpirun launches tasks using some mechanism other than Slurm, such as SSH (Not recommended).

> For more information look [here](https://slurm.schedmd.com/mpi_guide.html). {.is-info}

### PMI/MPIx API

On Rostam the SLURM is compiled with support for both `PMI2` and `PMIx` API

````bash
$ srun --mpi=list
MPI plugin types are...
	pmix
	cray_shasta
	none
	pmi2
specific pmix plugin versions available: pmix_v2
````

> In Rostam's `slurm.conf` the default MPI is set as `MpiDefault=pmi2
`
> Users can override the default API by `srun --mpi=` or equivalent environment variable `SLURM_MPI_TYPE` {.is-success}

### OpenMPI
Starting with OpenMPI version 3.1, PMIx version 2 is natively supported. To launch Open MPI application using PMIx version 2 the `--mpi=pmix_v2` option must be specified on the srun command line. OpenMPI version 4.0, adds support for PMIx version 3 and is invoked in the same way, with `--mpi=pmix_v3`.

Also on Rostam, the OpenMPI is configured with _--with-pmi_. To use the PMI API the option `--mpi=pmi2` or `--mpi=pmi2_v2` must be specified on the srun command line.

In summary, you can use:

````bash
salloc -n 20 sh   # allocates 20 cores and spawns shell for job
srun my_mpi_app
exit   # exits shell spawned by initial salloc command
````

Or much better:

````bash
srun -n 20 my_mpi_app
````

To use the PMI2 support

````bash
srun --mpi=pmi2 -n 20 my_mpi_app
````


To use the PMIx support

````bash
srun --mpi=pmix -n 20 my_mpi_app
````

### MPICH2

MPICH2 jobs can be launched using the `srun` command using pmi.

````bash
mpicc -lpmi2 ...
srun --mpi=pmi2 -n 20 my_mpi_app
````

