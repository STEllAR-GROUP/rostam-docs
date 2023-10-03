---
title: Hardware Resources
description: 
published: true
date: 2023-10-03
tags: 
editor: markdown
---

Rostam Cluster consists of 62 nodes with rainbow of hardware ranging from Intel's Skylake to AMD Rome, with Nvidia V100 and A100 to AMD MI100 GPUs.

## Hardware Summary

|Name       |Cores              |Memory |Accelerator    |Role   |Number |
|-----------|:-----------------:|:-----:|:-------------:|:-----:|:-----:|
|Buran      |48 (Rome)          |256 GB |None           |Compute|16     |
|Medusa     |40 (Skylake)       |96 GB  |None           |Compute|16     |
|Marvin     |16 (Sandy Bridge)  |48 GB  |None           |Compute|16     |
|Anvil      |128 (Rome)         |2 TB   |8 x A100       |Compute|1      |
|Nasrin     |128 (Rome)         |512 GB |2 x A100       |Compute|2      |
|Kamand     |128 (Rome)         |512 GB |2 x MI100      |Compute|2      |
|Toranj     |64 (Icelake)       |256 GB |4 x A100       |Compute|2      |
|Diablo     |40 (Skylake)       |386 GB |4 x V100       |Compute|1      |
|Geev       |20 (Haswell)       |256 GB |1 x V100       |Compute|1      |
|Bahram     |20 (Haswell)       |128 GB |1 x V100       |Compute|1      |
|DrStrange  |16 (Haswell)       |64 GB  |None           |Storage|1      |
|Troy       |16 (Sandy Bridge)  |64 GB  |None           |Storage|1      |
|Rostam1    |24 (Skylake)       |96 GB  |None           |Login  |1      |
|Marjorie   |48 (Rome)          |128 GB |None           |Head   |1      |

> All nodes have Hyper-threading off.
{.is-info}

## Interconnect

As the msn tool of communication between the nodes, Rostam uses HDR (200Gbps) and FDR(56Gbps) infiniband connectivity with fat tree topology.

Rostam also has a 25Gb SFP28 ethernet switch and a 1Gb copper cable ehternet switch to use on the nodes withouth Inifiniband connectivity.

## Storage
DrStrange is our main storage server configured with ZFS. It uses ten 14TB 7.2K 12Gb SAS disks divided into two RAID-Z1 VDEVs, with two 16GB PCIe Intel Optane for write buffer.
