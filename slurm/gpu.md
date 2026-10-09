---
title: Running GPU Jobs
description: 
published: true
date: 2026-10-09T23:52:13.085Z
tags: 
editor: markdown
dateCreated: 2023-10-03T20:20:46.397Z
---

# Running GPU Jobs

Rostam provides NVIDIA A100 and V100 GPUs, NVIDIA GB10 accelerators, and AMD Instinct MI100 GPUs. Slurm manages resource allocations. GPU software and architecture vary; check [Hardware Resources](/en/cluster/hardware) and [Software Availability by System](/en/software/availability) before building or running a GPU application.

## GPU systems and partitions

| Family | GPU | Nodes | GPUs per node | Primary partitions |
|---|---|---:|---:|---|
| Anvil | NVIDIA A100 | 1 | 8 | `DGX-A100`, `cuda` |
| Nasrin | NVIDIA A100 | 2 | 2 | `cuda-A100-amd`, `cuda-A100`, `cuda` |
| Toranj | NVIDIA A100 | 2 | 4 | `cuda-A100-intel`, `cuda-A100`, `cuda` |
| Diablo | NVIDIA V100 | 1 | 4 | `cuda-V100`, `cuda` |
| Bahram | NVIDIA V100 | 1 | 2 | `cuda-V100`, `cuda` |
| Kamand | AMD MI100 | 2 | 2 | `mi100` |
| Spark | NVIDIA GB10 | 4 | 1 | `spark` |

The four Spark nodes are individual ARM64 DGX Spark systems, interconnected for distributed jobs; they are **not** one four-GPU shared-memory server. Some nodes appear in multiple Slurm partitions; they are counted once physically.

## Discover resources

```bash
sinfo -o '%P %a %l %D %G'
sinfo -N -o '%N %P %t %G'
scontrol show node anvil
```

GRES fields reflect Slurm configuration, not physical device health or software compatibility.

## NVIDIA GPU allocations

NVIDIA devices are scheduled using Slurm Generic Resources (GRES). Explicitly request GPUs; `--exclusive` alone is **not** a substitute for requesting GPUs.

| Option | Meaning |
|---|---|
| `--gres=gpu:N` | N GPUs per node |
| `--gpus=N` | N GPUs for the whole job |
| `--gpus-per-node=N` | N GPUs per node |
| `--gpus-per-task=N` | N GPUs per task (with compatible Slurm selection setup) |

Interactive example requesting two A100 GPUs on an Anvil-compatible partition:

```bash
srun -p DGX-A100 -N 1 --gres=gpu:2 -t 01:00:00 --pty /bin/bash -l
nvidia-smi -L
```

`CUDA_VISIBLE_DEVICES` may remap GPU indices inside an allocation. Device visibility, GPU allocation, and exclusive node ownership are distinct concepts.

### Multi-task batch job

Save as `gpu-example.sh`:

```bash
#!/bin/bash
#SBATCH --job-name=gpu-example
#SBATCH --output=gpu-example-%j.out
#SBATCH --partition=DGX-A100
#SBATCH --nodes=1
#SBATCH --ntasks=4
#SBATCH --time=00:10:00
#SBATCH --gpus-per-task=2

srun bash -c 'echo "task=$SLURM_PROCID devices=$CUDA_VISIBLE_DEVICES"; nvidia-smi -L'
```

Submit:

```bash
sbatch gpu-example.sh
```

This requires eight schedulable GPUs on one node and per-task GPU scheduling support. The output of `nvidia-smi -L` depends on device mapping and Slurm restrictions.

## AMD MI100 allocations

Kamand has two MI100 GPUs per node. Historically, Rostam has allocated AMD GPU resources by node rather than individual GPU GRES. Confirm current `scontrol show node kamand0` output before assuming per-device isolation.

```bash
srun -p mi100 -N 1 -t 01:00:00 --exclusive --pty /bin/bash -l
rocm-smi
```

If the node is exclusively allocated and both drivers are operational, both MI100 devices should be visible. Seeing a device does not by itself prove individual-device isolation.

## DGX Spark

Each `spark-00` through `spark-03` node contains one GB10 accelerator and 128 GB unified local memory. For example:

```bash
salloc -p spark -N 4 -t 01:00:00
```

This requests four separate nodes, not four GPUs in one host. Distributed execution requires applications designed to communicate across systems. Inspect the node configuration before assuming a particular GRES syntax for Spark.

## Software environments

| GPU | Programming ecosystem |
|---|---|
| V100/A100/GB10 | NVIDIA CUDA |
| MI100 | AMD ROCm/HIP |

```bash
module spider cuda
module spider rocm
module list
nvidia-smi      # NVIDIA allocated nodes
rocm-smi        # AMD allocated nodes
```

The version printed by `nvidia-smi` is **not necessarily** the installed CUDA compiler version. Check `nvcc --version` after loading a compatible toolkit. For NVIDIA V100 and A100, CUDA compute capabilities are 7.0 and 8.0 respectively. The MI100 HIP target is `gfx908`. Toolkit, driver, and host GCC compatibility must be considered together. The NVIDIA HPC SDK has **not** been installed as a supported Rostam software environment.

## Monitoring and responsible use

```bash
squeue -u "$USER"
scontrol show job "$SLURM_JOB_ID"
sacct -j "$SLURM_JOB_ID" --format=JobID,State,Elapsed,AllocTRES
```

Only request the accelerators needed; test with short jobs before long submissions; monitor memory use and utilization; do not run GPU work directly on login nodes. For repeatability, record module versions, driver/runtime versions, allocated node types, and build options.

More information: [Slurm GRES](https://slurm.schedmd.com/gres.html), [Software Environment](/en/cluster/software), [Hardware Resources](/en/cluster/hardware).
