---
title: Hardware Resources
description: 
published: true
date: 2020-06-23T12:04:20.043Z
tags: 
editor: markdown
---

Rostam Cluster Consists of 49 nodes ranging from Intel's Skylake with Nvidia V100 to Raspberry Pi.

## Hardware Summary

|Name       |Cores              |Memory |Accelerator    |Role   |Number |
|-----------|:-----------------:|:-----:|:-------------:|:-----:|:-----:|
|Medusa     |40 (Skylake)       |96 GB  |None           |Compute|16     |
|Marvin     |16 (Sandy Bridge)  |48 GB  |None           |Compute|16     |
|Diablo     |40 (Skylake)       |386 GB |4 x V100       |Compute|1      |
|Geev       |20 (Haswell)       |256 GB |2 x V100       |Compute|1      |
|Bahram     |20 (Haswell)       |128 GB |2 x K80        |Compute|1      |
|Carson     |20 (Ivy Bridge)    |128 GB |2 x K80        |Compute|1      |
|Reno       |20 (Ivy Bridge)    |128 GB |1 x K40        |Compute|1      |
|RPi4       |4 (Cortex-A72)     |4 GB   |None           |Compute|4      |
|RPi3       |4 (Cortex-A53)     |1 GB   |None           |Compute|4      |
|AncientOne |16 (Sandy Bridge)  |64 GB  |None           |Storage|1      |
|DrStrange  |16 (Haswell)       |64 GB  |None           |Storage|1      |
|Rostam0    |16 (Sandy Bridge)  |128 GB |None           |Login  |1      |
|Caravaggio |24 (Skylake)       |96 GB  |None           |Head   |1      |

> All nodes have Hyper-threading off.
{.is-info}

## Network

Rostam uses a 25Gb switch as its main network system. All nodes except Marvin and Raspberry PIs are connected to the main switch. Marvin is a blade system and has its own 1Gb switch, which is connected with a 10Gb uplink to the main switch.

### Infiniband

Rostam also has a FDR(56Gbps) infiniband connectivity with fat tree topology. (In human language it means the two switches are connected with a 160Gbps connection).

There is no TCP/IP stack and therefore IP address on infiniband ports. If you want your application to use infiniband network, it should utilize `ibverbs` library. If your application uses TCP/IP, it is not using infiniband, it is using the ethernet switch. We intentionally configured the cluster this way to distinguish between the two networks. MPI understands this seperation and uses proper libraries when available.

## Storage
DrStrange is our main storage server configured with ZFS. It uses ten 14TB 7.2K 12Gb SAS disks devided into two RAID-Z1 VDEVs, with two 16GB PCIe Intel Optane for write buffer.
