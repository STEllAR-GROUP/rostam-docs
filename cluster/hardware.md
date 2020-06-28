---
title: Hardware Resources
description: 
published: true
date: 2020-06-28T22:55:00.392Z
tags: 
editor: markdown
---

Rostam Cluster Consists of 53 nodes ranging from Intel's Skylake with Nvidia V100 to Raspberry Pi.

## Hardware Summary

|Name       |Cores              |Memory |Accelerator    |Role   |Number |
|-----------|:-----------------:|:-----:|:-------------:|:-----:|:-----:|
|Medusa     |40 (Skylake)       |96 GB  |None           |Compute|16     |
|Marvin     |16 (Sandy Bridge)  |48 GB  |None           |Compute|16     |
|Diablo     |40 (Skylake)       |386 GB |4 x V100       |Compute|1      |
|Geev       |20 (Haswell)       |256 GB |2 x V100       |Compute|1      |
|Bahram     |20 (Haswell)       |128 GB |4 x K80        |Compute|1      |
|Carson     |20 (Ivy Bridge)    |128 GB |2 x R9 Fury    |Compute|1      |
|Reno       |20 (Ivy Bridge)    |128 GB |K40 + R9 Fury  |Compute|1      |
|Furiosa    |16 (Sandy Bridge)  |64 GB  |None           |Compute|1      |
|RPi4       |4 (Cortex-A72)     |4 GB   |None           |Compute|4      |
|RPi3+      |4 (Cortex-A53)     |1 GB   |None           |Compute|4      |
|RPi3       |4 (Cortex-A53)     |1 GB   |None           |Compute|4      |
|DrStrange  |16 (Haswell)       |64 GB  |None           |Storage|1      |
|Rostam0    |16 (Sandy Bridge)  |128 GB |None           |Login  |1      |
|Caravaggio |24 (Skylake)       |96 GB  |None           |Head   |1      |

> All nodes have Hyper-threading off.
{.is-info}

## Interconnect

As the msn tool of communication between the nodes, Rostam uses FDR(56Gbps) infiniband connectivity with fat tree topology. (In human language it means the two switches are connected with a 160Gbps connection).

Rostam also has a 25Gb SFP28 ethernet switch and a 1Gb copper cable ehternet switch to use on the nodes withouth Inifiniband connectivity.

## Storage
DrStrange is our main storage server configured with ZFS. It uses ten 14TB 7.2K 12Gb SAS disks devided into two RAID-Z1 VDEVs, with two 16GB PCIe Intel Optane for write buffer.
