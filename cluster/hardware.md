---
title: Hardware Resources
description: 
published: true
date: 2026-10-09T23:51:14.817Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:41.792Z
---

# Hardware Resources

Rostam is a heterogeneous HPC cluster with Intel and AMD x86-64, ARM64, and RISC-V processors. GPU systems include NVIDIA A100, V100, and GB10 and AMD MI100. Slurm schedules computational jobs across these physical systems.

## Physical systems

| System | Cores/node | Memory/node | Accelerator/node | Role | Nodes |
|---|---:|---|---|---|---:|
| Buran | 48 (AMD EPYC Rome) | 256 GB | None | Compute | 16 |
| Medusa | 40 (Intel Skylake) | 96 GB | None | Compute | 16 |
| Marvin | 16 (Intel Sandy Bridge) | 48 GB | None | Compute | 16 |
| Sven | 64 (RISC-V) | 125.9 GiB | None | Compute | 2 |
| Spark | 20 (ARM Cortex-X925/A725) | 128 GB unified | 1 × NVIDIA GB10 | Compute | 4 |
| Anvil | 128 (AMD EPYC Rome) | 2 TB | 8 × NVIDIA A100 | Compute | 1 |
| Nasrin | 128 (AMD EPYC Rome) | 512 GB | 2 × NVIDIA A100 | Compute | 2 |
| Kamand | 128 (AMD EPYC Rome) | 512 GB | 2 × AMD MI100 | Compute | 2 |
| Toranj | 64 (Intel Ice Lake) | 256 GB | 4 × NVIDIA A100 | Compute | 2 |
| Diablo | 40 (Intel Skylake) | 386 GB | 4 × NVIDIA V100 | Compute | 1 |
| Bahram | 20 (Intel Haswell) | 128 GB | 2 × NVIDIA V100 | Compute | 1 |
| Rostam1 | 24 (Intel Skylake) | 96 GB | None | Login | 1 |
| Jeremy | 64 (AMD EPYC 9354) | 250.9 GiB | None | Slurm controller | 1 |
| DrStrange | 16 (Intel Haswell) | 64 GB | None | ZFS storage | 1 |
| Volga | 48 (Intel Xeon Gold 6248R) | 375.9 GiB | None | Storage/NFS | 1 |

Marjorie (48 AMD EPYC Rome cores, 128 GB) is documented separately; it was not represented as an active Slurm compute node in the supplied inventory. Geev is not included because it does not appear in the current Slurm node list.

## Compute totals

| Resource | Installed count |
|---|---:|
| CPU-only Slurm compute nodes | 50 |
| GPU Slurm compute nodes | 13 |
| **Total Slurm compute nodes** | **63** |
| NVIDIA A100 GPUs | 20 |
| NVIDIA V100 GPUs | 6 |
| AMD MI100 GPUs | 4 |
| NVIDIA GB10 accelerators | 4 |
| **Total accelerators** | **34** |

These represent physical installed resources, not currently idle nodes. A physical node can appear in several partitions without adding hardware.

## GPU inventory and Slurm partitions

| System | GPU | Nodes | GPUs/node | Total GPUs | Partitions |
|---|---|---:|---:|---:|---|
| Anvil | A100 | 1 | 8 | 8 | `cuda`, `DGX-A100` |
| Nasrin | A100 | 2 | 2 | 4 | `cuda`, `cuda-A100`, `cuda-A100-amd` |
| Toranj | A100 | 2 | 4 | 8 | `cuda`, `cuda-A100`, `cuda-A100-intel` |
| Diablo | V100 | 1 | 4 | 4 | `cuda`, `cuda-V100` |
| Bahram | V100 | 1 | 2 | 2 | `cuda`, `cuda-V100` |
| Kamand | MI100 | 2 | 2 | 4 | `mi100` |
| Spark | GB10 | 4 | 1 | 4 | `spark` |
| **Total** | | **13** | | **34** | |

Four DGX Spark nodes, `spark-00` through `spark-03`, are interconnected for distributed computing; the network does not make them a single shared-memory four-GPU server. Each Spark has one GB10 and 128 GB unified local memory.

## Slurm partition overview

| Partition | Physical nodes | Maximum time |
|---|---:|---|
| `medusa` (default) | 16 | 3 days |
| `buran` | 16 | 3 days |
| `marvin` | 16 | 10 days |
| `risc5` | 2 | 3 days |
| `cuda` | 7 | 1 day |
| `cuda-A100` | 4 | 3 days |
| `cuda-A100-amd` | 2 | 3 days |
| `cuda-A100-intel` | 2 | 3 days |
| `cuda-V100` | 2 | 3 days |
| `DGX-A100` | 1 | 3 days |
| `mi100` | 2 | 1 day |
| `spark` | 4 | 3 days |

Specialized overlapping partitions include `sc15`, `jenkins-compute`, `jenkins-cuda`, `jenkins-A100`, and `jenkins-amdgpu`. They reference nodes already counted above. Partitions and limits can change; consult Slurm for current details.

```bash
sinfo
sinfo -N -o '%N %P %t %G'
scontrol show partition cuda-A100
scontrol show node nasrin0
```

## Networking

Rostam uses HDR (up to 200 Gbps) and FDR (up to 56 Gbps) InfiniBand in a fat-tree design, plus 25 Gb SFP28 and 1 Gb copper Ethernet for selected nodes, management, and storage connectivity. Link type and bandwidth vary by node; not every system has the same interconnect. The Spark systems also have an interconnection supporting distributed communication and applicable RDMA-capable transfers, without creating shared GPU memory.

## Storage and infrastructure

**DrStrange** is a ZFS storage server with ten 14 TB 7.2K SAS disks arranged into two RAID-Z1 VDEVs and two 16 GB Intel Optane PCIe devices used in its storage configuration. Raw capacity is not the same as usable space.

**Volga** provides active storage and NFS services. **Rostam1** provides login access through `rostam.cct.lsu.edu`. **Jeremy** provides Slurm controller/infrastructure services. Login and infrastructure systems are not general-purpose Slurm compute nodes.

## Access and software

Compute-intensive applications belong in Slurm allocations, not on login hosts. See [Running GPU Jobs](/en/slurm/gpu) for GPU allocation examples, [Software Availability by System](/en/software/availability) for the software matrix, and [Connecting to Rostam](/en/ssh) for SSH instructions.

*Inventory: supplied Slurm configuration and hardware data, October 9, 2026. Check `sinfo` for live state.*
