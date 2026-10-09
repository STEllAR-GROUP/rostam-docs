---
title: Hardware Resources
description: 
published: true
date: 2026-10-09T22:19:36.800Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:41.792Z
---


# Rostam Hardware

Rostam is a heterogeneous high-performance computing (HPC) cluster operated by the LSU Center for Computation & Technology (CCT). It supports scientific computing, software development, computational research, and accelerator-based workloads across multiple processor architectures.

The cluster includes Intel x86-64, AMD x86-64, ARM64, and RISC-V systems, together with NVIDIA V100, A100, and GB10 accelerators and AMD Instinct MI100 GPUs.

Rostam's compute resources are managed through the Slurm workload manager. Users can select among CPU architectures, GPU types, and specialized partitions according to their computational requirements.

## Hardware Summary

The following inventory describes Rostam's physical computing systems. Hardware counts represent installed resources rather than current scheduling availability.

| System | CPU Cores/Node | Memory/Node | Accelerator/Node | Role | Nodes |
|---|---:|---:|---|---|---:|
| Buran | 48 (AMD EPYC Rome) | 256 GB | None | CPU Compute | 16 |
| Medusa | 40 (Intel Skylake) | 96 GB | None | CPU Compute | 16 |
| Marvin | 16 (Intel Sandy Bridge) | 48 GB | None | CPU Compute | 16 |
| Sven | 64 (RISC-V) | 125.9 GiB | None | CPU Compute | 2 |
| Spark | 20 (ARM Cortex-X925/A725) | 128 GB unified | 1 × NVIDIA GB10 | GPU Compute | 4 |
| Anvil | 128 (AMD EPYC Rome) | 2 TB | 8 × NVIDIA A100 | GPU Compute | 1 |
| Nasrin | 128 (AMD EPYC Rome) | 512 GB | 2 × NVIDIA A100 | GPU Compute | 2 |
| Kamand | 128 (AMD EPYC Rome) | 512 GB | 2 × AMD MI100 | GPU Compute | 2 |
| Toranj | 64 (Intel Ice Lake) | 256 GB | 4 × NVIDIA A100 | GPU Compute | 2 |
| Diablo | 40 (Intel Skylake) | 386 GB | 4 × NVIDIA V100 | GPU Compute | 1 |
| Bahram | 20 (Intel Haswell) | 128 GB | 2 × NVIDIA V100 | GPU Compute | 1 |
| DrStrange | 16 (Intel Haswell) | 64 GB | None | Storage | 1 |
| Volga | 48 (Intel Xeon Gold 6248R) | 375.9 GiB | None | Storage / NFS | 1 |
| Rostam1 | 24 (Intel Skylake) | 96 GB | None | Login | 1 |
| Jeremy | 64 (AMD EPYC 9354) | 250.9 GiB | None | Slurm Controller / Infrastructure | 1 |

**Additional system:** Marjorie is registered in the Rostam environment but is not currently included in the active Slurm compute inventory. Its previously documented configuration is 48 AMD EPYC Rome cores and 128 GB RAM.

> **CPU topology:** The documented compute systems generally expose one hardware thread per physical core. Hyper-threading or simultaneous multithreading is disabled or otherwise not exposed to the operating system on these systems. Spark's ARM processor uses a heterogeneous core architecture.

## Compute Resource Summary

Rostam currently includes the following physical resources in its Slurm compute inventory:

| Resource | Total |
|---|---:|
| CPU-only compute nodes | 50 |
| GPU compute nodes | 13 |
| Total Slurm compute nodes | 63 |
| NVIDIA A100 GPUs | 20 |
| NVIDIA V100 GPUs | 6 |
| AMD MI100 GPUs | 4 |
| NVIDIA GB10 accelerators | 4 |
| **Total GPU accelerators** | **34** |

Node counts include systems that may be allocated, drained, or unavailable for scheduling.

The four DGX Spark systems are counted as four physical compute nodes, each containing one GB10 accelerator.

## CPU Compute Systems

Rostam provides several CPU architectures for application development, portability testing, and production scientific workloads.

### AMD EPYC Systems

The Buran nodes provide 48 AMD EPYC Rome cores and 256 GB of memory per node.

These systems are suitable for CPU-intensive workloads, MPI applications, software compilation, and general-purpose scientific computing.

Buran consists of:

```text
buran00 through buran15
```

All 16 systems belong to the `buran` Slurm partition.

### Intel Xeon Systems

The Medusa nodes provide 40 Intel Skylake cores and 96 GB of memory per node.

Medusa consists of:

```text
medusa00 through medusa15
```

The Marvin nodes provide 16 Intel Sandy Bridge cores and 48 GB of memory per node.

Marvin consists of:

```text
marvin00 through marvin15
```

These systems provide additional CPU capacity and support compatibility testing across multiple x86-64 processor generations.

### RISC-V Systems

Rostam includes two RISC-V compute nodes:

```text
sven0
sven1
```

Each system provides 64 RISC-V CPU cores and approximately 125.9 GiB of memory.

The Sven systems are available through the `risc5` partition.

These nodes support research involving RISC-V software development, architecture portability, compiler toolchains, and native application execution.

Because RISC-V differs from x86-64 and ARM64, applications may require architecture-specific builds and dependencies.

## GPU Compute Systems

Rostam provides GPU acceleration using multiple NVIDIA and AMD accelerator architectures.

GPU-equipped systems support workloads such as numerical simulations, scientific visualization, parallel computing, and accelerator programming.

### GPU Hardware Inventory

| System | Accelerator | Nodes | GPUs/Node | Total GPUs | Slurm Partition(s) |
|---|---|---:|---:|---:|---|
| Anvil | NVIDIA A100 | 1 | 8 | 8 | `cuda`, `DGX-A100` |
| Nasrin | NVIDIA A100 | 2 | 2 | 4 | `cuda`, `cuda-A100`, `cuda-A100-amd` |
| Toranj | NVIDIA A100 | 2 | 4 | 8 | `cuda`, `cuda-A100`, `cuda-A100-intel` |
| Diablo | NVIDIA V100 | 1 | 4 | 4 | `cuda`, `cuda-V100` |
| Bahram | NVIDIA V100 | 1 | 2 | 2 | `cuda`, `cuda-V100` |
| Kamand | AMD MI100 | 2 | 2 | 4 | `mi100` |
| Spark | NVIDIA GB10 | 4 | 1 | 4 | `spark` |
| **Total** | | **13** | | **34** | |

The same physical nodes may appear in multiple Slurm partitions. These partition memberships do not represent additional hardware.

### NVIDIA A100

Rostam's NVIDIA A100 resources are distributed across Anvil, Nasrin, and Toranj.

- **Anvil:** One DGX-class system with eight A100 GPUs and 2 TB of system memory.
- **Nasrin:** Two AMD EPYC-based systems, each containing two A100 GPUs.
- **Toranj:** Two Intel Ice Lake-based systems, each containing four A100 GPUs.

Together, these systems provide 20 NVIDIA A100 accelerators.

NVIDIA A100 systems support CUDA-based scientific computing and GPU-accelerated applications.

### NVIDIA V100

Rostam provides six NVIDIA V100 accelerators across two systems:

- **Diablo:** Four V100 GPUs.
- **Bahram:** Two V100 GPUs.

Both systems are accessible through the `cuda-V100` partition.

The V100 architecture provides an additional target for CUDA application development, compatibility testing, and GPU computing.

### AMD Instinct MI100

The Kamand systems provide AMD Instinct MI100 accelerators.

Rostam includes:

```text
kamand0
kamand1
```

Each node contains two MI100 GPUs, providing four AMD accelerators in total.

The systems are accessible through the `mi100` Slurm partition.

AMD GPU development uses the ROCm software ecosystem, including HIP and supported numerical libraries.

Application and toolchain compatibility depends on the installed ROCm environment.

### NVIDIA DGX Spark

Rostam includes four NVIDIA DGX Spark systems:

```text
spark-00
spark-01
spark-02
spark-03
```

Each DGX Spark contains:

- One NVIDIA GB10 Grace Blackwell Superchip.
- A 20-core ARM processor combining Cortex-X925 and Cortex-A725 cores.
- One integrated NVIDIA Blackwell GPU.
- 128 GB of unified system memory.

These four systems are interconnected to support communication between nodes and distributed computing workloads.

The `spark` Slurm partition contains all four systems.

Although the four Sparks collectively provide four GB10 accelerators, they remain four separate physical nodes. Their interconnection does not make them equivalent to one node with four GPUs or one unified memory space.

Distributed applications must use an appropriate communication model to execute across multiple Spark systems.

For example, an allocation requesting all four Spark nodes can be submitted using:

```bash
salloc -p spark -N 4 -t 01:00:00
```

The request is granted according to node availability and Slurm scheduling policies.

## Slurm Partitions

Rostam uses Slurm to schedule computational work and manage resource allocation.

Partitions group nodes according to hardware characteristics, intended use, or workload requirements.

### General Compute Partitions

| Partition | Nodes | Hardware | Time Limit |
|---|---:|---|---|
| `medusa` | 16 | Intel Skylake CPU | 3 days |
| `buran` | 16 | AMD EPYC Rome CPU | 3 days |
| `marvin` | 16 | Intel Sandy Bridge CPU | 10 days |
| `risc5` | 2 | RISC-V CPU | 3 days |

The default Slurm partition is `medusa`.

### GPU Partitions

| Partition | Nodes | Hardware | Time Limit |
|---|---:|---|---|
| `cuda` | 7 | NVIDIA A100/V100 systems | 1 day |
| `cuda-A100` | 4 | NVIDIA A100 systems | 3 days |
| `cuda-A100-amd` | 2 | Nasrin A100 systems | 3 days |
| `cuda-A100-intel` | 2 | Toranj A100 systems | 3 days |
| `cuda-V100` | 2 | NVIDIA V100 systems | 3 days |
| `DGX-A100` | 1 | Anvil DGX A100 | 3 days |
| `mi100` | 2 | AMD MI100 systems | 1 day |
| `spark` | 4 | NVIDIA GB10 systems | 3 days |

### Specialized Partitions

Rostam also provides specialized partitions for automated testing and specific workloads.

| Partition | Hardware/Nodes | Purpose |
|---|---|---|
| `jenkins-compute` | Selected Buran, Medusa, and Marvin nodes | Automated CPU testing |
| `jenkins-cuda` | Toranj0, Bahram | CUDA testing |
| `jenkins-A100` | Toranj0, Nasrin0 | NVIDIA A100 testing |
| `jenkins-amdgpu` | Kamand0 | AMD GPU testing |
| `sc15` | Bahram | Specialized workload partition |

These partitions share physical systems with other partitions and should not be counted as additional compute nodes.

Partition time limits, node availability, and scheduling policies may change.

### Checking Current Slurm Configuration

List partitions and their current state:

```bash
sinfo
```

Display nodes individually:

```bash
sinfo -N -o "%N %P %t %G"
```

Display detailed information about a partition:

```bash
scontrol show partition <partition-name>
```

Inspect a particular compute node:

```bash
scontrol show node <node-name>
```

View currently running and queued jobs:

```bash
squeue
```

Show only your jobs:

```bash
squeue -u "$USER"
```

To view GPU-related partitions:

```bash
sinfo -p cuda,cuda-A100,cuda-V100,DGX-A100,mi100,spark
```

For details about GPU scheduling, see the cluster's GPU computing documentation and the [Slurm GRES documentation](https://slurm.schedmd.com/gres.html).

## Network and Interconnect

Rostam uses multiple networking technologies to support high-performance communication, storage access, cluster management, and external connectivity.

### InfiniBand

The primary high-performance computing fabric includes:

- HDR InfiniBand, supporting link speeds up to 200 Gbps.
- FDR InfiniBand, supporting link speeds up to 56 Gbps.

The InfiniBand infrastructure uses a fat-tree topology to provide high-bandwidth communication between connected systems.

InfiniBand is particularly useful for:

- MPI-based distributed computing.
- Low-latency communication.
- Large scientific simulations.
- RDMA-capable applications.
- High-throughput data movement.

Not every Rostam node necessarily has the same InfiniBand connectivity or supported link speed.

Application performance depends on the connectivity of the nodes allocated to a job.

### Ethernet

Rostam also uses Ethernet networking, including:

- 25 Gb SFP28 Ethernet connectivity.
- 1 Gb copper Ethernet connectivity.
- Dedicated management and infrastructure networks.

These networks serve systems without InfiniBand, storage and administrative services, and other cluster communication requirements.

The precise network path available to an application depends on the nodes involved and their configured interfaces.

### DGX Spark Interconnect

The four DGX Spark systems are interconnected for communication between the GB10-based nodes.

Their interconnection supports distributed workloads and direct-memory-access-capable communication where supported by the networking and software stack.

Each Spark retains its own CPU, GPU, memory, and operating-system environment.

Applications running across multiple Sparks must explicitly support distributed execution; the network does not automatically combine the four systems into a single accelerator or shared memory system.

## Storage

Rostam provides shared storage infrastructure for user files, software, and computational data.

Storage services are distinct from compute resources and may use different filesystems, performance characteristics, and retention policies.

### DrStrange

DrStrange is a ZFS-based storage server.

Its documented storage configuration includes:

- Ten 14 TB, 7,200 RPM, 12 Gb SAS hard drives.
- Two RAID-Z1 virtual devices (VDEVs).
- Two 16 GB PCIe Intel Optane devices associated with write acceleration.

The disk configuration provides approximately 140 TB of raw hard-drive capacity before RAID-Z redundancy, filesystem overhead, and other reservations.

Usable capacity is lower.

ZFS provides data integrity features, checksumming, and flexible storage management.

### Volga

Volga is an active storage and NFS server.

Its hardware configuration includes:

- 48 Intel Xeon Gold 6248R CPU cores.
- Approximately 375.9 GiB of system memory.
- Network-attached storage services.

Volga provides storage and filesystem services within the Rostam environment.

### Shared Filesystems

Shared filesystems allow users to access their files from multiple cluster systems without manually copying data between every compute node.

Users should distinguish between:

- Home directories for user files and configuration.
- Shared application and software directories.
- Working storage for computational input and output.
- Temporary or scratch storage, where available.

Storage locations may have different capacity, performance, quota, and backup policies.

Users should consult cluster storage documentation before placing large datasets or high-volume temporary output in shared home directories.

## Login and Management Infrastructure

### Rostam1

Rostam1 provides the user login environment through which users access Rostam.

The public SSH entry point is:

```text
rostam.cct.lsu.edu
```

Login systems are intended for:

- SSH access.
- File management.
- Source code editing.
- Lightweight compilation.
- Software environment preparation.
- Slurm job submission and monitoring.

CPU-intensive and GPU-intensive production workloads should be submitted to Slurm compute nodes rather than executed directly on login infrastructure.

### Jeremy

Jeremy is an AMD EPYC 9354-based infrastructure system.

Its documented hardware includes:

- Two AMD EPYC 9354 processors.
- 64 physical CPU cores.
- Approximately 250.9 GiB of system memory.
- No NVIDIA GPU.

Jeremy provides Slurm controller and other cluster infrastructure services.

It is not part of the general-purpose Slurm compute inventory.

### Marjorie

Marjorie is documented as an AMD EPYC Rome system with 48 CPU cores and 128 GB of memory.

It is not currently represented as an active compute node in the Slurm partitions listed above.

Its role and availability should be distinguished from the scheduled compute inventory.

## Software Environments

Rostam uses environment modules, including Lmod, to manage scientific applications, compilers, libraries, and development tools.

The cluster's heterogeneous hardware means that software compatibility can differ between systems.

Users should verify compiler, library, and GPU runtime compatibility before executing workloads across different architectures.

To list available modules:

```bash
module avail
```

To search for a particular package:

```bash
module spider <package>
```

To inspect loaded modules:

```bash
module list
```

Rostam supports multiple CPU and accelerator architectures, but the availability of a particular software module does not guarantee compatibility with every node.

For GPU applications, supported CUDA and ROCm versions must be compatible with the driver and runtime environment of the allocated system.

## Resource Availability

The hardware inventory describes the cluster's installed resources, not the number of nodes currently available for new jobs.

Slurm nodes can be in several states, including:

| State | Meaning |
|---|---|
| `idle` | Available for scheduling |
| `alloc` | Allocated to running jobs |
| `mix` | Partially allocated |
| `drain` | Not accepting new allocations |
| `down` | Unavailable |
| `down*` | Unavailable or not responding |

A node listed in the hardware inventory may be temporarily unavailable because of maintenance, hardware problems, software updates, or existing job allocations.

For current availability, consult Slurm directly:

```bash
sinfo
```

## Additional Documentation

- [Accessing Rostam](/ssh)
- [Rostam GPU Computing](/cluster/gpu)
- [Slurm Workload Manager](https://slurm.schedmd.com/documentation.html)
- [Slurm GRES Documentation](https://slurm.schedmd.com/gres.html)
- [NVIDIA CUDA Documentation](https://docs.nvidia.com/cuda/)
- [AMD ROCm Documentation](https://rocm.docs.amd.com/)
- [Lmod Documentation](https://lmod.readthedocs.io/)
