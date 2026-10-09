---
title: Running GPU Jobs
description: 
published: true
date: 2026-10-09T19:16:13.887Z
tags: 
editor: markdown
dateCreated: 2023-10-03T20:20:46.397Z
---


# GPU Computing on Rostam

Rostam provides heterogeneous GPU computing resources supporting scientific research, high-performance computing, and software development. These resources include AMD Instinct MI100 and NVIDIA V100 and A100 accelerators.

Consistent with the principles of *Magnifica Humanitas*, Rostam is operated as a shared research environment in which computing resources should be used responsibly, efficiently, and reproducibly. Users are encouraged to request only the resources needed for their work and to consider the portability of their applications across different computing architectures.

For a complete inventory of available systems, see the [Rostam Hardware](/cluster/hardware) page.

## GPU Architectures and Software Environments

| Architecture | GPU | Primary Software Ecosystem |
|---|---|---|
| AMD CDNA | MI100 | ROCm, HIP |
| NVIDIA Volta | V100 | CUDA |
| NVIDIA Ampere | A100 | CUDA |

Not all GPU applications are portable between AMD and NVIDIA architectures. CUDA applications generally require NVIDIA GPUs, while HIP applications may support multiple architectures when built appropriately.

GPU software availability depends on installed drivers, compiler toolchains, runtime libraries, and environment modules.

Users should verify the software environment available on their allocated nodes before running production workloads.

## Discovering Available GPU Resources

Rostam uses Slurm to manage job scheduling and resource allocation.

To list partitions and their configured generic resources:

```bash
sinfo -o "%P %a %l %D %G"
```

To inspect resources advertised by individual nodes:

```bash
sinfo -N -o "%N %P %t %G"
```

For additional information about a particular node:

```bash
scontrol show node <nodename>
```

These commands report Slurm's configured resources. They do not necessarily establish whether a GPU is physically healthy, whether its driver is operational, or whether the required software environment is available.

Some AMD GPU resources may not appear in the GRES listing if their allocation is managed at the node level rather than through individual GPU resources.

## AMD GPU Computing

Rostam includes AMD Instinct MI100 GPU nodes.

AMD GPU access on Rostam has traditionally been managed through node allocation rather than individual GPU GRES allocation. Under this configuration, an allocation on an AMD GPU node may expose all GPUs installed in that node.

### Interactive AMD GPU Session

To request an interactive session on an MI100 node:

```bash
srun -p mi100 -N 1 -t 1:00:00 \
    --exclusive --pty /bin/bash -l
```

Once the job starts, inspect the available GPUs:

```bash
rocm-smi
```

On a node equipped with two MI100 GPUs, the command should identify both devices, provided the ROCm driver stack is functioning.

Example output:

```text
======================= ROCm System Management Interface =======================
================================= Concise Info =================================
GPU  Temp (DieEdge)  AvgPwr  SCLK    MCLK     Fan  Perf  PwrCap  VRAM%  GPU%
0    35.0c           34.0W   300Mhz  1200Mhz  0%   auto  290.0W    0%   0%
1    32.0c           34.0W   300Mhz  1200Mhz  0%   auto  290.0W    0%   0%
================================================================================
============================= End of ROCm SMI Log ==============================
```

> **Important:** GPU visibility does not necessarily imply exclusive GPU ownership. Users should not assume that GPU devices are isolated unless the node allocation and Slurm configuration guarantee it.

The `--exclusive` option requests exclusive node allocation, subject to Slurm's configuration and scheduling policies.

### AMD Development Environments

ROCm provides the software ecosystem for AMD GPU computing, including:

- HIP programming and runtime support
- GPU-aware numerical and mathematical libraries
- OpenMP GPU offloading through compatible compilers
- Profiling and debugging utilities
- GPU management and monitoring tools

Available software may be provided through environment modules.

To discover ROCm-related modules:

```bash
module avail
module spider rocm
```

Module availability and compatibility can vary between systems. The presence of an MI100 GPU does not necessarily guarantee that every ROCm development component is installed.

## NVIDIA GPU Computing

Rostam provides NVIDIA V100 and A100 GPU resources, including DGX-class systems.

NVIDIA GPU allocation is managed through Slurm's **Generic Resource (GRES) scheduling**.

Users must explicitly request GPU resources when submitting jobs to GPU partitions.

> **Important:** Running a job with `--exclusive` does not replace an explicit GPU resource request. GPU resources should be requested using the appropriate Slurm GPU allocation options.

### GPU Allocation Options

| Slurm Option | Purpose |
|---|---|
| `--gres=gpu:N` | Request N GPUs per node |
| `--gpus=N` | Request N GPUs for the job |
| `--gpus-per-node=N` | Request N GPUs per node |
| `--gpus-per-socket=N` | Request N GPUs per socket |
| `--gpus-per-task=N` | Request N GPUs per task |

The `--gpus*` options require compatible Slurm resource-selection configuration, typically `select/cons_tres`.

### Example 1: Interactive NVIDIA GPU Session

Request an interactive session on an A100 DGX node with two GPUs:

```bash
srun -p DGX-A100 -N 1 -t 1:00:00 \
    --gres=gpu:2 \
    --pty /bin/bash -l
```

Once the interactive job begins:

```bash
nvidia-smi -L
```

Example output:

```text
GPU 0: NVIDIA A100-SXM4-80GB (UUID: GPU-...)
GPU 1: NVIDIA A100-SXM4-80GB (UUID: GPU-...)
```

Slurm may use GPU device filtering and environment variables such as `CUDA_VISIBLE_DEVICES` to restrict which devices are available to individual job steps.

The GPUs visible to an application may therefore represent a subset of the physical GPUs installed on the node.

### Exclusive Node Allocation Versus GPU Allocation

The `--exclusive` option requests exclusive node allocation. It does not replace an explicit GPU resource request.

For example:

```bash
srun -p DGX-A100 -N 1 -t 1:00:00 \
    --exclusive --gres=gpu:2 \
    --pty /bin/bash -l
```

This requests exclusive use of the node while explicitly allocating two GPUs.

Slurm's accounting and resource-allocation behavior determines the exact CPU, memory, and GPU resources assigned to the job.

Exclusive node allocation does not necessarily mean every GPU on the node has been requested or allocated to the job.

Users should avoid requesting exclusive node access unless it is required for their workload.

### Example 2: Multi-GPU Batch Job

The following example requests four tasks on a single A100 node, with two GPUs assigned to each task.

Create a file named `my-gpu-job.sh`:

```bash
#!/bin/bash
#SBATCH --job-name=gpu-example
#SBATCH --output=gpu-example-%j.out
#SBATCH --partition=DGX-A100
#SBATCH --nodes=1
#SBATCH --ntasks=4
#SBATCH --time=00:10:00
#SBATCH --gpus-per-task=2

srun bash -c '
    echo "Task: ${SLURM_PROCID}"
    echo "Visible GPUs: ${CUDA_VISIBLE_DEVICES}"
    nvidia-smi -L
'
```

Submit the job:

```bash
sbatch my-gpu-job.sh
```

Example submission:

```text
Submitted batch job 165315
```

View the results:

```bash
cat gpu-example-165315.out
```

Each task should be assigned two GPUs.

The example requires a node with at least eight allocatable GPUs and a Slurm configuration supporting per-task GPU allocation.

GPU numbering displayed inside a job may differ from physical device numbering because of device visibility and remapping.

For additional information about GPU resource allocation, consult the [Slurm GRES documentation](https://slurm.schedmd.com/gres.html#Running_Jobs).

## GPU Software and Environment Modules

Rostam uses environment modules, including Lmod, to manage software installations and compiler environments.

Modules allow users to select available versions of compilers, numerical libraries, GPU development toolkits, and other scientific software without requiring changes to system-wide installations.

### Discovering Available Software

List available modules:

```bash
module avail
```

Search for CUDA-related modules:

```bash
module spider cuda
```

Search for ROCm-related modules:

```bash
module spider rocm
```

Search for compiler modules:

```bash
module spider gcc
```

Module availability may differ between nodes and software environments.

The availability of a module does not guarantee compatibility with every GPU node. Software may be installed only on selected systems or require particular driver versions.

### CUDA

CUDA is NVIDIA's parallel computing platform and programming environment.

The CUDA ecosystem provides:

- GPU compilation and runtime support
- CUDA C and C++ development tools
- GPU-accelerated mathematical libraries
- Profiling and debugging utilities
- APIs for GPU execution and memory management

CUDA applications should be compiled for GPU architectures supported by the intended execution nodes.

NVIDIA V100 and A100 GPUs have different compute capabilities.

| GPU | CUDA Compute Capability |
|---|---|
| NVIDIA V100 | 7.0 |
| NVIDIA A100 | 8.0 |

CUDA compiler architecture flags may need to be adjusted for the target GPU.

For example, when using an available NVIDIA CUDA compiler:

```bash
nvcc -arch=sm_70 example.cu -o example-v100
```

For an A100:

```bash
nvcc -arch=sm_80 example.cu -o example-a100
```

These examples assume that the CUDA compiler is available in the selected software environment.

Users should verify the installed CUDA toolkit and GPU driver versions before compiling applications.

### ROCm and HIP

ROCm is AMD's GPU computing software platform.

HIP provides a C++ programming interface for GPU applications and supports source-level portability between supported AMD and NVIDIA GPU environments.

Portability does not necessarily imply binary compatibility. Applications may need to be compiled separately for each target architecture.

AMD MI100 GPUs use the `gfx908` architecture target.

For example, with an available HIP compiler:

```bash
hipcc --offload-arch=gfx908 example.cpp -o example-mi100
```

This command requires a compatible ROCm installation and HIP compiler.

### OpenMP GPU Offloading

OpenMP provides a directive-based approach to parallel programming and includes features for offloading selected computational regions to accelerator devices.

OpenMP GPU offloading support depends on the compiler, runtime libraries, and GPU architecture.

Compiler support for CPU parallelism does not automatically imply support for GPU offloading.

Users should verify that their selected compiler was built with support for the intended accelerator target.

### Compiler and Driver Compatibility

Compilers, GPU development toolkits, runtime libraries, and device drivers serve different purposes.

GPU drivers are installed and maintained at the operating-system level. They provide the kernel and device interfaces required to communicate with GPU hardware.

Development toolkits and compiler environments provide the software used to compile, link, and execute GPU applications.

Loading a CUDA or ROCm environment module does not ordinarily install or replace the system's kernel-level GPU driver.

Applications must be compatible with the driver and runtime environment of their allocated nodes.

Users should not assume that applications built with a particular compiler or toolkit version will run identically across every GPU architecture.

Similarly, loading a newer GCC module does not necessarily change the compiler used to build an existing GPU application or replace the compiler required by a particular CUDA or ROCm installation.

## Monitoring GPU Utilization

GPU monitoring helps users understand application behavior and determine whether allocated resources are being used effectively.

### NVIDIA GPU Monitoring

Display NVIDIA GPU status:

```bash
nvidia-smi
```

Monitor GPU utilization continuously:

```bash
nvidia-smi -l 5
```

List visible GPUs:

```bash
nvidia-smi -L
```

Useful metrics include:

- GPU utilization
- GPU memory consumption
- Power consumption
- Temperature
- Active GPU processes

### AMD GPU Monitoring

Display AMD GPU status:

```bash
rocm-smi
```

Monitor device status periodically:

```bash
watch -n 5 rocm-smi
```

ROCm monitoring provides information about GPU utilization, device temperatures, memory consumption, and power usage, depending on hardware and driver support.

### Slurm Job Information

Display information about a running job:

```bash
scontrol show job <jobid>
```

View recent job accounting:

```bash
sacct -j <jobid> \
    --format=JobID,JobName,Partition,State,Elapsed,AllocTRES
```

Slurm GPU accounting availability depends on the cluster's configured accounting and monitoring plugins.

## Responsible Use of Shared GPU Resources

Rostam's GPU systems are shared research infrastructure. Efficient scheduling and appropriate resource requests help ensure that these systems remain accessible to the broader research community.

Users should follow these practices:

1. **Request only the GPUs required.** Avoid allocating additional GPUs unless an application can make effective use of them.

2. **Avoid unnecessary exclusive allocations.** Exclusive node access can prevent other jobs from using otherwise available resources.

3. **Test workloads before production execution.** Short interactive or batch jobs can identify software compatibility and performance issues before large allocations are requested.

4. **Monitor resource utilization.** Low GPU utilization may indicate that an application is limited by CPU performance, memory transfers, storage, or other factors.

5. **Use appropriate partitions.** Select GPU partitions according to application requirements and hardware compatibility.

6. **Preserve reproducibility.** Record software versions, module selections, and job configurations so computational results can be reproduced.

7. **Respect the scheduler.** GPU-intensive work should be executed through Slurm allocations rather than directly on shared login or management nodes.

These practices improve overall cluster utilization while supporting equitable access to computational resources.

## Reproducibility and Portability

Because Rostam contains multiple CPU and GPU architectures, applications may behave differently depending on the hardware and software environment used.

Researchers are encouraged to document their execution environments, including:

- GPU model and architecture
- GPU driver version
- CUDA or ROCm version
- Compiler and runtime versions
- Loaded environment modules
- Slurm partition and resource requests
- Application build options
- Relevant performance and execution parameters

### Recording the Software Environment

Display loaded modules:

```bash
module list
```

Display information about the current Slurm job:

```bash
scontrol show job "$SLURM_JOB_ID"
```

For NVIDIA systems:

```bash
nvidia-smi
```

For AMD systems:

```bash
rocm-smi
```

Capturing this information improves reproducibility and helps administrators diagnose compatibility and performance issues.

### Architecture-Aware Compilation

NVIDIA V100 and A100 accelerators implement different GPU architectures and compute capabilities.

Similarly, AMD MI100 GPUs use an architecture and software ecosystem distinct from NVIDIA accelerators.

Applications compiled for one architecture are not necessarily compatible with another.

Where practical, users should build their applications for the specific accelerator architecture on which they intend to execute.

For applications targeting multiple systems, separate architecture-specific builds or appropriately configured portable programming models may be necessary.

## Additional Documentation

- [Rostam Hardware Inventory](/cluster/hardware)
- [Slurm GRES Documentation](https://slurm.schedmd.com/gres.html)
- [Slurm GPU Scheduling](https://slurm.schedmd.com/gres.html#Running_Jobs)
- [Slurm sbatch Documentation](https://slurm.schedmd.com/sbatch.html)
- [Slurm srun Documentation](https://slurm.schedmd.com/srun.html)
- [AMD ROCm Documentation](https://rocm.docs.amd.com/)
- [AMD HIP Documentation](https://rocm.docs.amd.com/projects/HIP/en/latest/)
- [NVIDIA CUDA Documentation](https://docs.nvidia.com/cuda/)
- [Lmod Documentation](https://lmod.readthedocs.io/)
