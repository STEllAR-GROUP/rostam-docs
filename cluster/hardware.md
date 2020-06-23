---
title: Hardware Architecture
description: 
published: true
date: 2020-06-23T00:48:21.027Z
tags: 
editor: markdown
---

Rostam Cluster Consists of 45 nodes ranging from Intel's Skylake with Nvidia V100 to Raspberry Pi.

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
