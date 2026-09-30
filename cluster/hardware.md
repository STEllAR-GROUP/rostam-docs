---
title: Hardware Resources
description: 
published: true
date: 2026-09-30T14:35:24.464Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:41.792Z
---

Rostam Cluster consists of a heterogeneous mix of Intel- and AMD-based systems, with NVIDIA V100 and A100 GPUs and AMD MI100 GPUs.

## Hardware Summary

|Name       |Cores              |Memory |Accelerator    |Role   |Number |
|-----------|:-----------------:|:-----:|:-------------:|:-----:|:-----:|
|Buran      |48 (Rome)          |256 GB |None           |Compute|16     |
|Medusa     |40 (Skylake)       |96 GB  |None           |Compute|16     |
|Marvin     |16 (Sandy Bridge)  |48 GB  |None           |Compute|16     |
|Sven       |TBD                |TBD    |TBD            |Compute|2      |
|Spark      |TBD                |TBD    |TBD            |Compute|4      |
|Anvil      |128 (Rome)         |2 TB   |8 x A100       |Compute|1      |
|Nasrin     |128 (Rome)         |512 GB |2 x A100       |Compute|2      |
|Kamand     |128 (Rome)         |512 GB |2 x MI100      |Compute|2      |
|Toranj     |64 (Ice Lake)      |256 GB |4 x A100       |Compute|2      |
|Diablo     |40 (Skylake)       |386 GB |4 x V100       |Compute|1      |
|Geev       |20 (Haswell)       |256 GB |2 x n300d      |Compute|1      |
|Bahram     |20 (Haswell)       |128 GB |2 x V100       |Compute|1      |
|DrStrange  |16 (Haswell)       |64 GB  |None           |Storage|1      |
|Volga      |TBD                |TBD    |None           |Storage|1      |
|Rostam1    |24 (Skylake)       |96 GB  |None           |Login  |1      |

> Existing documented nodes have Hyper-threading disabled. Hyper-threading status for newly added systems should be verified.
{.is-info}

## Interconnect

As the main communication fabric between nodes, Rostam uses HDR (200 Gbps) and FDR (56 Gbps) InfiniBand connectivity with a fat-tree topology.

Rostam also has a 25 Gb SFP28 Ethernet switch and a 1 Gb copper Ethernet switch for nodes without InfiniBand connectivity.

## Storage

DrStrange is configured as a ZFS storage server. It uses ten 14 TB 7.2K 12 Gb SAS disks divided into two RAID-Z1 VDEVs, with two 16 GB PCIe Intel Optane devices for write buffering.

Volga is also used for storage and NFS services. Its hardware configuration should be documented after verification.