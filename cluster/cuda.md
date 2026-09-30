---
title: CUDA Programming
description: 
published: true
date: 2026-09-30T18:15:38.913Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:39.607Z
---

# GPU Programming

Rostam provides several NVIDIA and AMD GPU systems for accelerator and GPU programming. GPU hardware varies significantly between nodes, so users should select an appropriate Slurm partition and verify the software environment on the allocated node.

## GPU Hardware

Current GPU-capable systems include:

|System|Accelerator|Count per node|Slurm partition(s)|
|---|---|---:|---|
|Anvil|NVIDIA A100|8|`cuda`, `DGX-A100`|
|Nasrin|NVIDIA A100|2|`cuda`, `cuda-A100`, `cuda-A100-amd`|
|Toranj|NVIDIA A100|4|`cuda`, `cuda-A100`, `cuda-A100-intel`|
|Diablo|NVIDIA V100|4|`cuda`, `cuda-V100`|
|Bahram|NVIDIA V100|2|`cuda`, `cuda-V100`|
|Kamand|AMD MI100|2|`mi100`|
|Spark|NVIDIA GB10|1|`spark`|

Additional specialized partitions may exist for automated testing or administrative workloads.

## NVIDIA CUDA

CUDA is used on Rostam's NVIDIA GPU systems.

CUDA consists of two distinct layers:

- The **NVIDIA driver**, which is installed and managed at the operating-system level.
- The **CUDA toolkit**, which provides `nvcc`, CUDA headers, runtime libraries, and development tools.

A node can use one NVIDIA driver while supporting multiple CUDA toolkit versions. Users should therefore not assume that the CUDA version shown by `nvidia-smi` is the CUDA toolkit currently selected.

To examine the NVIDIA driver and GPU:

```bash
nvidia-smi
```

To determine the active CUDA toolkit:

```bash
nvcc --version
```

or:

```bash
nvcc -V
```

To see CUDA modules available in the current environment:

```bash
module avail cuda
```

and to see currently loaded modules:

```bash
module list
```

> The `CUDA Version` reported by `nvidia-smi` indicates the CUDA level supported by the installed NVIDIA driver. It does not necessarily indicate which CUDA toolkit is installed or loaded.

## Selecting a CUDA Environment

Where multiple CUDA toolkits are available, select the desired version using the module system.

For example:

```bash
module load cuda/<version>
```

Then verify the environment:

```bash
nvcc --version
nvidia-smi
```

Available CUDA versions may differ as the cluster software stack is updated. Use `module avail cuda` rather than assuming a particular version is installed.

## Running on an NVIDIA GPU

For general NVIDIA GPU access:

```bash
srun -p cuda --gres=gpu:1 --pty bash
```

For V100 systems:

```bash
srun -p cuda-V100 --gres=gpu:1 --pty bash
```

For A100 systems:

```bash
srun -p cuda-A100 --gres=gpu:1 --pty bash
```

More specific A100 partitions are available when CPU architecture matters:

```text
cuda-A100-intel
cuda-A100-amd
DGX-A100
```

Users should consult the Slurm documentation for current scheduling options and resource-request syntax.

## AMD MI100

The Kamand nodes provide AMD MI100 accelerators.

These systems use the AMD GPU software stack rather than NVIDIA CUDA and are available through the:

```text
mi100
```

partition.

Installed ROCm compiler and library versions should be checked on the allocated node rather than assumed.

Useful commands include:

```bash
rocm-smi
```

and:

```bash
module avail rocm
```

if ROCm modules are provided.

## Spark

The `spark` partition contains four ARM-based systems with NVIDIA GB10 accelerators.

Because these systems differ substantially from the x86_64 CUDA nodes in both CPU and GPU architecture, software built for other Rostam GPU nodes should not automatically be assumed to be binary-compatible with Spark.

Users should build and test software specifically for the Spark environment.

## Checking an Allocated Node

After obtaining a GPU node, the following commands provide a useful summary of the environment:

```bash
hostname
module list
nvidia-smi
nvcc --version
```

For AMD GPU nodes:

```bash
hostname
module list
rocm-smi
```

## Compatibility

GPU software compatibility depends on several components:

- GPU architecture
- NVIDIA or AMD driver version
- CUDA or ROCm toolkit version
- host compiler version
- application and library requirements

In particular, older NVIDIA GPUs such as the V100 may require different toolkit choices from newer GPU architectures. Users developing portable GPU applications should test against the specific GPU generations they intend to use.

For current hardware details, see the [Hardware Resources](/en/cluster/hardware) page.