---
title: Software Availability by System
description: 
published: true
date: 2026-10-09T23:37:58.102Z
tags: 
editor: markdown
dateCreated: 2026-10-09T23:37:58.102Z
---


# Software Availability by System

Rostam provides compilers, programming languages, scientific libraries, MPI implementations, and GPU development environments across multiple hardware architectures.

This page summarizes software availability by node family to help users choose an appropriate computing environment.

For instructions on using Lmod, managing Python environments, and understanding compiler compatibility, see [Software Environment](/software).

> **Inventory date:** October 9, 2026.
>
> The tables summarize software detected through system commands, installed packages, administrator-managed software directories, and Lmod module discovery. Versions represent available installations rather than necessarily loaded defaults. Individual nodes may differ.

## Software Availability at a Glance

### CPU Compute Systems

| System | Nodes | OS | CPU Architecture | System GCC | Additional Software |
|---|---:|---|---|---|---|
| Buran | 16 | AlmaLinux 9.6 | AMD EPYC Rome | 11.5 | GCC 12–16, Clang, Intel oneAPI, Python, Open MPI, Boost, CMake, HDF5, NetCDF, Apptainer |
| Medusa | 16 | AlmaLinux 9.6 | Intel Skylake | 11.5 | GCC 12–16, Clang, Intel oneAPI, Python, Open MPI, Boost, CMake, HDF5, NetCDF, Apptainer |
| Marvin | 16 | AlmaLinux 9.6 | Intel Sandy Bridge | 11.5 | GCC 12–16, Clang, Intel oneAPI, Python, Open MPI, Boost, CMake, HDF5, NetCDF, Apptainer |
| Sven | 2 | Fedora 38 | RISC-V | 13.2 | Clang 16, Python 3.11, CMake, Open MPI, UCX, PMIx, hwloc, Apptainer |

Buran, Medusa, and Marvin share a broadly similar administrator-managed x86-64 software environment.

The RISC-V systems maintain a separate native software environment. Applications may require architecture-specific compilation.

### GPU Compute Systems

| System | Nodes | OS | Accelerator | System GCC | Additional Software |
|---|---:|---|---|---|---|
| Anvil | 1 | AlmaLinux 9.6 | 8 × NVIDIA A100 | 11.5 | CUDA, GCC 12–16, Clang, Intel oneAPI, Open MPI, Boost, CMake, Apptainer |
| Nasrin | 2 | AlmaLinux 9.6 | 2 × NVIDIA A100/node | 11.5 | CUDA, GCC 12–16, Clang, Intel oneAPI, Open MPI, Boost, CMake, Apptainer |
| Toranj | 2 | AlmaLinux 9.6 | 4 × NVIDIA A100/node | 11.5 | CUDA, GCC 12–16, Clang, Intel oneAPI, Open MPI, Boost, CMake, Apptainer |
| Diablo | 1 | AlmaLinux 9.6 | 4 × NVIDIA V100 | 11.5 | CUDA, GCC 12–16, Clang, Intel oneAPI, Open MPI, Boost, CMake, Apptainer |
| Bahram | 1 | AlmaLinux 9.6 | 2 × NVIDIA V100 | 11.5 | CUDA, GCC 12–16, Clang, Intel oneAPI, Open MPI, Boost, CMake, Apptainer |
| Kamand | 2 | AlmaLinux 9.6 | 2 × AMD MI100/node | 11.5 | ROCm 6.4, HIP, GCC 12–16, Clang, Open MPI, Boost, CMake, Apptainer |
| Spark | 4 | Ubuntu 24.04 | 1 × NVIDIA GB10/node | 13.3 | CUDA 13.0, GCC 16.2, Python 3.12, Open MPI 5.0.10, Boost 1.88, CMake 4.0 |

GPU software availability and application compatibility depend on GPU architecture, driver version, compiler support, and installed runtime libraries.

The four DGX Spark systems use ARM64 processors and maintain a separate software environment from the x86-64 GPU nodes.

### Login and Infrastructure Systems

| System | Operating System | System GCC | Role |
|---|---|---|---|
| Rostam1 | AlmaLinux 9.7 | 11.5 | Login and job submission |
| Jeremy | Not collected | — | Slurm controller and infrastructure |
| Sapphire | RHEL 9.7 | 11.5 | Application and web infrastructure |
| Volga | RHEL 9.6 | 11.5 | Storage and NFS |
| DrStrange | RHEL 9.6 | 11.5 | ZFS storage |
| Marjorie | Not collected | — | Infrastructure |

Infrastructure software environments are not necessarily intended for general computational work.

## Compilers and Development Tools

Rostam provides multiple compiler toolchains to support applications with different language, architecture, and compatibility requirements.

### GCC

| Platform | System GCC | Additional GCC |
|---|---|---|
| Main x86-64 compute nodes | 11.5.0 | GCC Toolsets 12–14; Lmod GCC 16.1/16.2 and other versions |
| Sven (RISC-V) | 13.2.1 | Architecture-specific toolchains |
| Spark (ARM64) | 13.3.0 | GCC 16.2.0 |

On the main x86-64 environment, GCC 16.2.0 is available through Lmod:

```bash
module load gcc/16.2.0
```

Confirm the active compiler:

```bash
gcc --version
```

Different libraries may depend on different GCC versions. Users should verify module compatibility before mixing toolchains.

### LLVM and Clang

The main AlmaLinux 9.6 compute systems include LLVM/Clang 19.1.7 as a system-installed toolchain.

Rostam1 provides a newer system Clang release, while the RISC-V systems report Clang 16.0.6.

Additional compiler installations may be available through Lmod.

To find installed Clang modules:

```bash
module spider clang
```

### CMake and Build Utilities

The main AlmaLinux 9.6 systems provide CMake 3.26.5 as a system-installed version.

Additional versions, including 3.29.2 and 4.0.0, are present in the administrator-managed x86-64 software collection.

The Spark systems also expose CMake 4.0 through their software environment.

To find versions:

```bash
module spider cmake
```

Other build utilities include GNU Make, binutils, and Ninja where installed.

### Intel oneAPI

Intel oneAPI components are installed on the main x86-64 compute environment.

The surveyed package inventory includes components of the 2025.2 development toolkits, including C/C++ and Fortran compilers, numerical libraries, and development utilities.

Installed components include software from:

- Intel oneAPI compiler toolchains.
- Intel MPI.
- Intel oneMKL.
- Intel oneTBB.
- Intel oneDNN.
- Intel performance and development tools.

Availability through environment modules depends on the selected software configuration.

Intel oneAPI installation does not imply that every component supports every processor architecture or workload.

## Python

Python versions vary by operating system and module environment.

| Platform | System Python | Additional Python |
|---|---|---|
| Main AlmaLinux 9.6 compute nodes | 3.9.21 | 3.12.3, 3.13.2 |
| Rostam1 | 3.9.23 | Additional module-managed versions |
| Sven | 3.11.5 | Architecture-specific installations |
| Spark | 3.12.3 | Architecture-specific installations |

Users should generally select a Python version appropriate for their application and create a project-specific virtual environment.

To find Python modules:

```bash
module spider python
```

To load Python 3.12 on the main x86-64 environment:

```bash
module load python/3.12.3
```

To create a virtual environment:

```bash
python3 -m venv ~/venvs/myproject
source ~/venvs/myproject/bin/activate
```

Install packages with:

```bash
python -m pip install numpy scipy
```

Python environments containing compiled packages may not be portable across x86-64, ARM64, and RISC-V systems.

## MPI and Parallel Computing

Rostam provides MPI implementations and supporting parallel communication libraries.

| Platform | MPI and Communication Software |
|---|---|
| Main x86-64 compute nodes | Open MPI, Intel MPI, UCX, PMIx, hwloc |
| Sven | Open MPI 5.0.3, UCX, PMIx, hwloc |
| Spark | Open MPI 5.0.10, UCX, PMIx |
| GPU x86-64 nodes | Open MPI and supporting libraries, subject to accelerator compatibility |

Additional versions may be installed or provided through the module hierarchy.

To discover Open MPI:

```bash
module spider openmpi
```

The selected MPI implementation should be compatible with the compiler and runtime libraries used to build the application.

Slurm manages resource allocation and job scheduling. Users should use the supported MPI process-launch configuration for their selected software environment.

## Scientific Libraries

The main x86-64 software collection provides a broad range of numerical, scientific, and development libraries.

| Category | Available Software |
|---|---|
| C++ libraries | Boost |
| Numerical computing | Intel oneMKL and other scientific libraries |
| Scientific data formats | HDF5, NetCDF, NetCDF-Fortran |
| Parallel data formats | PnetCDF, PIO |
| Parallel communications | Open MPI, UCX, PMIx, GASNet |
| Hardware topology | hwloc |
| Performance analysis | PAPI, LIKWID, OTF2 |
| Build tools | CMake, GNU Make, Ninja |
| Containers | Apptainer |

Not every package is available on every architecture, and some installations depend on specific compilers or prerequisite libraries.

### Boost

The administrator-managed x86-64 environment includes multiple Boost versions, including releases through 1.92.0.

The Spark environment includes Boost 1.88.

To discover available versions and prerequisites:

```bash
module spider boost
```

For a specific release:

```bash
module spider boost/1.92.0-release
```

C++ libraries should generally be built with compatible compiler and standard library environments.

## GPU Development Environments

### NVIDIA CUDA

CUDA environments are provided for NVIDIA GPU systems.

| Systems | GPU | Software |
|---|---|---|
| Anvil, Nasrin, Toranj | A100 | NVIDIA CUDA toolkit environments |
| Diablo, Bahram | V100 | NVIDIA CUDA toolkit environments |
| Spark | GB10 | CUDA 13.0 |

The A100 and V100 software collections include several CUDA toolkit installations. Users should inspect Lmod for versions available on their allocated systems.

```bash
module spider cuda
```

After loading a suitable toolkit:

```bash
nvcc --version
```

To inspect the NVIDIA driver and GPU:

```bash
nvidia-smi
```

The driver-supported CUDA version and installed CUDA toolkit version are not necessarily identical.

### AMD ROCm

The Kamand systems provide AMD MI100 accelerators and ROCm software.

The October 2026 survey detected ROCm 6.4 on the Kamand nodes.

ROCm provides runtime, compiler, and library support for AMD GPU programming.

To inspect GPU status:

```bash
rocm-smi
```

To search for available software modules:

```bash
module spider rocm
```

ROCm and HIP application compatibility depends on the installed runtime, compiler, and GPU architecture.

### ARM64 NVIDIA GB10

The four DGX Spark systems provide NVIDIA GB10 accelerators and a separate ARM64 development environment.

Their software collection includes:

- CUDA 13.0.
- GCC 16.2.0.
- Python 3.12.
- Open MPI 5.0.10.
- Boost 1.88.
- CMake 4.0.
- UCX and PMIx.

Software compiled for the main x86-64 systems will generally require rebuilding for ARM64.

The four Spark nodes are separate Slurm compute nodes, even though they are interconnected.

## Containers

Apptainer is installed on the main x86-64 compute environment, with version 1.4.2 detected during the survey.

Check availability:

```bash
apptainer --version
```

Container images must be compatible with the host processor architecture.

GPU-enabled containers must also be compatible with the host's GPU drivers and runtime environment.

## Using Environment Modules

Rostam uses Lmod to provide access to multiple software installations and versions.

To list modules currently available:

```bash
module avail
```

To search the complete module hierarchy:

```bash
module spider
```

To find a specific package:

```bash
module spider gcc
```

To load a module:

```bash
module load gcc/16.2.0
```

To see loaded modules:

```bash
module list
```

To unload all modules:

```bash
module purge
```

Some software libraries are compiler-dependent and become accessible only after loading the required compiler or environment.

See [Software Environment](/software) for detailed instructions.

## Software Availability and Compatibility

Software availability depends on several factors:

1. **Architecture:** x86-64, ARM64, and RISC-V systems require compatible executables and libraries.
2. **Compiler:** Libraries and applications may depend on a particular compiler or ABI.
3. **Dependencies:** Some Lmod modules require other modules to be loaded first.
4. **GPU support:** CUDA and ROCm toolkits must support the selected GPU and driver configuration.
5. **Scheduling:** A node may have the required software installed but be temporarily unavailable through Slurm.

To check available nodes:

```bash
sinfo
```

To inspect the software environment of an allocated node:

```bash
module avail
```

Use `module spider` to discover modules that are not immediately visible.

## Inventory Coverage

The October 9, 2026 software survey collected detailed information from 66 of 69 targeted systems.

Three systems did not return complete results:

| Host | Inventory Status |
|---|---|
| Jeremy | SSH connection refused |
| Marjorie | SSH authentication denied |
| Marvin00 | Hostname resolution failure |

The Marvin family summary is based on the responding Marvin systems.

The survey records installed packages, system compiler versions, detected libraries, Lmod configuration, module discovery output, and administrator-managed software directories.

A detected installation does not necessarily guarantee successful compilation or execution of every application that depends on it.

## Further Documentation

- [Software Environment](/software)
- [Rostam Hardware Inventory](/cluster/hardware)
- [Accessing Rostam](/ssh)
- [Advanced SSH Configuration](/ssh/advanced_ssh)
- [Lmod Documentation](https://lmod.readthedocs.io/)
- [Slurm Workload Manager](https://slurm.schedmd.com/documentation.html)
- [NVIDIA CUDA Documentation](https://docs.nvidia.com/cuda/)
- [AMD ROCm Documentation](https://rocm.docs.amd.com/)
