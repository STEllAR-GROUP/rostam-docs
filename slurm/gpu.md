---
title: Running GPU Jobs
description: 
published: true
date: 2023-10-05T13:25:08.753Z
tags: 
editor: markdown
dateCreated: 2023-10-03T20:20:46.397Z
---

Rostam has GPU nodes with AMD MI100 GPUs and Nvidia V100 and A100. Refer to [list of available hardware](/cluster/hardware)

## AMD GPU
There is no device limit on AMD GPU nodes, by running a job on any node equipped with an AMD GPU, the GPU would be available

For example; run an interactive session on a node with AMD MI100 GPU:
```bash
srun -p mi100 -N 1 -t 1:00:00 --exclusive --pty /bin/bash -l
```
The 2 GPUs would be available to use

```bash
$ rocm-smi 

======================= ROCm System Management Interface =======================
================================= Concise Info =================================
GPU  Temp (DieEdge)  AvgPwr  SCLK    MCLK     Fan  Perf  PwrCap  VRAM%  GPU%  
0    35.0c           34.0W   300Mhz  1200Mhz  0%   auto  290.0W    0%   0%    
1    32.0c           34.0W   300Mhz  1200Mhz  0%   auto  290.0W    0%   0%    
================================================================================
============================= End of ROCm SMI Log ==============================
```
## Nvidia GPU

Access to Nvidia GPUs are controlled by SLURM's *Generic Resource (GRES) Scheduling*

> Running a job with or without `--exclusive` on a with with Nvidia GPU will **not** grant access to the GPU unless you explicitly specify the GPU resources by GRES options!{.is-warning}

Jobs will not be allocated any generic resources unless specifically requested at job submit time using the options:

- `--gres` Generic resources required per node
- `--gpus` GPUs required per job
- `--gpus-per-node` GPUs required per node. Equivalent to the `--gres` option for GPUs.
- `--gpus-per-socket` GPUs required per socket. Requires the job to specify a task socket.
- `--gpus-per-task` GPUs required per task (Process). Requires the job to specify a task count.

### Examples
**Example 1:** Running a an interactive job on DGX node with only to GPU available:
```bash
$ srun -p DGX-A100 -N 1 -t 1:00:00 --gres=gpu:2 --exclusive --pty /bin/bash -l
$ nvidia-smi -L
GPU 0: NVIDIA A100-SXM4-80GB (UUID: GPU-54188da3-123f-9d8d-af42-e5616c208a2a)
GPU 1: NVIDIA A100-SXM4-80GB (UUID: GPU-7e80f4cb-3657-1cfa-f97f-3d740a6694af)
```
Note that here `--exclusive` means, all cpu and memory resource on the node should be available to the job.

**Example 2:** run a batch job with four processes on one DGX node, each process get access two GPUs:

`my-gpu-job.sh`:
```bash
#!/bin/bash
#
#SBATCH --job-name=test_sbatch-gpu
#SBATCH --output=sbatch-gpu.out
#SBATCH --partition=DGX-A100
#SBATCH --ntasks=4
#SBATCH --nodes=1
#SBATCH --time=10:00
#SBATCH	--gpus-per-task=2

srun nvidia-smi -L
```
execute:

```bash
$ sbatch my-gpu-job.sh 
Submitted batch job 165315
```

see the output:
```bash
$ cat sbatch-gpu.out
GPU 0: NVIDIA A100-SXM4-80GB (UUID: GPU-efdbd648-3670-7c68-84e6-fccad4ae3de3)
GPU 1: NVIDIA A100-SXM4-80GB (UUID: GPU-a5524abc-0427-48a1-cc73-a00598e56906)
GPU 0: NVIDIA A100-SXM4-80GB (UUID: GPU-0cc8fd1b-d79a-5959-3e75-9e61cad5252b)
GPU 1: NVIDIA A100-SXM4-80GB (UUID: GPU-78693047-7798-197e-77b7-748cc6cfea98)
GPU 0: NVIDIA A100-SXM4-80GB (UUID: GPU-54188da3-123f-9d8d-af42-e5616c208a2a)
GPU 1: NVIDIA A100-SXM4-80GB (UUID: GPU-7e80f4cb-3657-1cfa-f97f-3d740a6694af)
GPU 0: NVIDIA A100-SXM4-80GB (UUID: GPU-0de68359-765d-a32e-09f9-8598b6b727ec)
GPU 1: NVIDIA A100-SXM4-80GB (UUID: GPU-1a96b9a9-3438-1ec5-5b45-397f3ff4c4cf)
```

> For more information look at `GRES` [documentation](https://slurm.schedmd.com/gres.html#Running_Jobs){.is-success}