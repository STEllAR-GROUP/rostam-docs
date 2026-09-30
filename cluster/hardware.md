---
title: Hardware Resources
description: 
published: true
date: 2026-09-30T15:01:23.624Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:41.792Z
---

Rostam Cluster consists of a heterogeneous mix of Intel-, AMD-, ARM-, and RISC-V-based systems, with NVIDIA V100, A100, and GB10 accelerators and AMD MI100 GPUs.

## Hardware Summary

|Name       |Cores                          |Memory     |Accelerator    |Role             |Number |
|-----------|:-----------------------------:|:---------:|:--------------:|:---------------:|:-----:|
|Buran      |48 (Rome)                      |256 GB     |None            |Compute          |16     |
|Medusa     |40 (Skylake)                   |96 GB      |None            |Compute          |16     |
|Marvin     |16 (Sandy Bridge)              |48 GB      |None            |Compute          |16     |
|Sven       |64 (RISC-V)                    |125.9 GiB  |None            |Compute          |2      |
|Spark      |20 (Cortex-X925 / Cortex-A725) |121.7 GiB  |NVIDIA GB10     |Compute          |4      |
|Anvil      |128 (Rome)                     |2 TB       |8 x A100        |Compute          |1      |
|Nasrin     |128 (Rome)                     |512 GB     |2 x A100        |Compute          |2      |
|Kamand     |128 (Rome)                     |512 GB     |2 x MI100       |Compute          |2      |
|Toranj     |64 (Ice Lake)                  |256 GB     |4 x A100        |Compute          |2      |
|Diablo     |40 (Skylake)                   |386 GB     |4 x V100        |Compute          |1      |
|Geev       |20 (Haswell)                   |256 GB     |2 x n300d       |Compute          |1      |
|Bahram     |20 (Haswell)                   |128 GB     |2 x V100        |Compute          |1      |
|DrStrange  |16 (Haswell)                   |64 GB      |None            |Storage          |1      |
|Volga      |48 (Xeon Gold 6248R)           |375.9 GiB  |None            |Storage          |1      |
|Rostam1    |24 (Skylake)                   |96 GB      |None            |Login            |1      |
|Marjorie   |48 (Rome)                      |128 GB     |None            |TBD              |1      |
|Jeremy     |64 (EPYC 9354)                 |250.9 GiB  |None            |Slurm Controller |1      |

> Existing documented compute nodes have Hyper-threading disabled. Jeremy, Volga, and the Spark nodes also report one hardware thread per core.
{.is-info}

## Interconnect

As the main communication fabric between nodes, Rostam uses HDR (200 Gbps) and FDR (56 Gbps) InfiniBand connectivity with a fat-tree topology.

Rostam also has a 25 Gb SFP28 Ethernet switch and a 1 Gb copper Ethernet switch for nodes without InfiniBand connectivity.

## Storage

DrStrange is configured as a ZFS storage server. It uses ten 14 TB 7.2K 12 Gb SAS disks divided into two RAID-Z1 VDEVs, with two 16 GB PCIe Intel Optane devices for write buffering.

Volga is also an active storage and NFS server.