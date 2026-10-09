---
title: Software Availability by System
description: 
published: true
date: 2026-10-09T23:50:58.165Z
tags: 
editor: markdown
dateCreated: 2026-10-09T23:37:58.102Z
---

# Software Availability by System

This is a quick-reference summary of software detected during the October 9, 2026 Rostam survey. Entries are grouped by node family; installed software and available modulefiles are not necessarily loaded by default or compatible in every combination.

For instructions on modules, compilers, Python, and MPI, see [Software Environment](/en/cluster/software). For the physical resources, see [Hardware Resources](/en/cluster/hardware).

## CPU compute nodes

| Family | Nodes | OS / architecture | System GCC | Representative available software |
|---|---:|---|---|---|
| Buran | 16 | AlmaLinux 9.6 / x86-64 | 11.5 | GCC toolsets and Lmod compilers through 16.2; LLVM/Clang, Intel oneAPI components, Python, Open MPI, Boost, CMake, NetCDF/HDF5, UCX, PMIx, Apptainer |
| Medusa | 16 | AlmaLinux 9.6 / x86-64 | 11.5 | Shared x86-64 modules: GCC, Clang, oneAPI, Python, MPI, Boost, CMake, scientific libraries, Apptainer |
| Marvin | 16 | AlmaLinux 9.6 / x86-64 | 11.5 | Shared x86-64 modules: GCC, Clang, oneAPI, Python, MPI, Boost, CMake, scientific libraries, Apptainer |
| Sven | 2 | Fedora 38 / RISC-V | 13.2 | Native RISC-V toolchains, Python 3.11, Open MPI, UCX/PMIx, hwloc, build utilities |

## GPU compute nodes

| Family | Nodes | OS / architecture | GPUs per node | Representative software |
|---|---:|---|---|---|
| Anvil | 1 | AlmaLinux 9.6 / x86-64 | 8 × A100 | NVIDIA drivers, CUDA installations/modules, GCC/Clang, MPI, Boost, x86-64 scientific stack |
| Nasrin | 2 | AlmaLinux 9.6 / x86-64 | 2 × A100 | NVIDIA drivers, CUDA installations/modules, GCC/Clang, MPI, scientific libraries |
| Toranj | 2 | AlmaLinux 9.6 / x86-64 | 4 × A100 | NVIDIA drivers, CUDA installations/modules, GCC/Clang, MPI, scientific libraries |
| Diablo | 1 | AlmaLinux 9.6 / x86-64 | 4 × V100 | NVIDIA driver, CUDA installations/modules, GCC/Clang, MPI, scientific libraries |
| Bahram | 1 | AlmaLinux 9.6 / x86-64 | 2 × V100 | NVIDIA driver, CUDA installations/modules, GCC/Clang, MPI, scientific libraries |
| Kamand | 2 | AlmaLinux 9.6 / x86-64 | 2 × MI100 | ROCm 6.4 identified, HIP-related tools, GCC/Clang, MPI, x86-64 scientific stack |
| Spark | 4 | Ubuntu 24.04 / ARM64 | 1 × GB10 | CUDA 13.0, GCC 13.3 system / 16.2 module, Python 3.12, Open MPI 5.0.10, Boost 1.88, CMake 4.0, UCX, PMIx |

The four Spark systems are separate ARM64 nodes (`spark-00` through `spark-03`), not one four-GPU node.

## Login and infrastructure

| System | Operating system | Purpose |
|---|---|---|
| Rostam1 | AlmaLinux 9.7 | SSH login / job submission |
| Jeremy | Not collected by remote survey | Slurm controller / infrastructure |
| Sapphire | RHEL 9.7 | Application and web infrastructure |
| Volga | RHEL 9.6 | Storage / NFS |
| DrStrange | RHEL 9.6 | ZFS storage |
| Marjorie | Not collected | Infrastructure / DNS registration |

## Software categories

| Category | Main x86-64 environment | Architecture-specific differences |
|---|---|---|
| C/C++/Fortran compilers | GCC 11.5 system; GCC toolsets, additional GCC/Clang and Intel oneAPI components | Sven and Spark have separate native toolchains |
| Build tools | CMake, Make, Ninja, binutils | Versions vary; Spark has CMake 4.0 modules |
| Python | 3.9 system; additional 3.12/3.13 installations | Sven: 3.11 system; Spark: 3.12 system |
| MPI/communication | Open MPI, Intel MPI components, UCX, PMIx, hwloc | Versions differ across architectures |
| C++ libraries | Boost through 1.92.0 in surveyed x86-64 module trees | Spark includes Boost 1.88 |
| Scientific I/O | HDF5, NetCDF, NetCDF-Fortran, PnetCDF, PIO | Availability is architecture and module dependent |
| GPU programming | CUDA on NVIDIA nodes; ROCm/HIP on Kamand | Spark CUDA 13.0; Kamand ROCm 6.4 identified |
| Containers | Apptainer detected on main x86-64 environment | Check availability before use on Sven/Spark |

## Find an exact version

```bash
module avail
module spider gcc
module spider python
module spider openmpi
module spider boost
module spider cuda
module spider rocm
```

A module advertised by `spider` may require prerequisite modules. For example:

```bash
module spider boost/1.92.0-release
module load gcc/16.2.0
module list
```

Do not assume GPU compiler and runtime compatibility solely from the presence of a modulefile. Test on the intended execution node.

## Survey limitations

The deep survey successfully collected 66 of 69 targeted hosts. The collection failed for `jeremy`, `marjorie`, and `marvin00`. The Marvin family summary is based on responding Marvin nodes. A software package seen in the inventory might be installed but not loaded, and some programs may require a compatible compiler module first.

For up-to-date node state:

```bash
sinfo -N -o '%N %P %t %G'
```

Related: [Software Environment](/en/cluster/software) · [Hardware Resources](/en/cluster/hardware) · [Running GPU Jobs](/en/slurm/gpu).
