---
title: Software Modules (lmod)
description: 
published: true
date: 2026-10-12T16:39:29.212Z
tags: 
editor: markdown
dateCreated: 2026-10-12T16:39:29.212Z
---

# Lmod Software Catalog by System

This reference lists **actual Lmod module names and advertised versions** discovered on Rostam on **October 9, 2026**, grouped by physical node family. It complements [Software Environment](/en/cluster/software) (how modules work), [Software Availability by System](/en/software/availability) (quick summary), and [Hardware Resources](/en/cluster/hardware) (hardware and scheduling).

**Interpretation:** Each `name/version` is a *discoverable module*, primarily from `module spider`, not necessarily loadable simultaneously or compatible with every compiler. Modules may be hidden by a hierarchical `MODULEPATH` until prerequisite modules are loaded. Entries are distinct from system RPM/DEB packages and manually installed files. The survey was read-only and did not test loading every module.

## How the software stacks fit together

| Layer / stack | What it provides | How to select it | Where observed |
|---|---|---|---|
| OS / system toolchain | Distribution GCC, glibc, system Python, base packages | Available without additional modules | All surveyed systems; versions differ by OS/architecture |
| GNU / GCC through Lmod | Alternate `gcc`, `g++`, `gfortran` and compiler-dependent libraries | `module spider gcc`; `module load gcc/<version>` | Main x86-64 nodes; separate native builds on Sven and Spark |
| LLVM / Clang through Lmod | Alternate Clang/LLVM compilation toolchain | `module spider llvm` | Main x86-64 module hierarchy |
| Intel oneAPI | Intel compilers, MPI, MKL and component libraries | `module spider intel`; component modules as applicable | Main x86-64 software hierarchy and login node |
| MPI / communication | Open MPI, Intel MPI, UCX, PMIx, hwloc; compiler-sensitive | Select a compatible MPI stack after choosing compiler | Architecture-specific versions on x86-64, ARM64 and RISC-V |
| Scientific libraries | Boost, HDF5, NetCDF, PIO, etc.; compiler-dependent | `module spider <package>` for prerequisite instructions | Mainly x86-64; smaller ARM64/RISC-V trees |
| NVIDIA CUDA | `nvcc`, CUDA runtime and libraries; requires compatible drivers | `module spider cuda` | Anvil, Nasrin, Toranj, Diablo, Bahram, Spark |
| AMD ROCm / HIP | AMD GPU compilation, runtime and libraries | `module spider rocm` | Kamand |
| **NVIDIA HPC SDK — possible future addition, not installed** | NVIDIA HPC compilers, GPU runtime/math/communication components, potentially OpenACC/OpenMP offload | Future `nvhpc*` modules after validation | **No surveyed host** |

**Toolchain hierarchy:** Operating-system software is the baseline. Loading a compiler can change module search paths, enabling libraries built with that compiler. Choosing MPI adds another compatibility layer; the GPU toolkit must also match the accelerator and driver. A module appearing in `spider` should not be taken to mean every combination works.

## Selecting a compatible environment

```bash
module purge
module spider gcc
module load gcc/16.2.0
module spider boost
module list
```

For GPU toolkits, check **on the target node**:

```bash
module spider cuda     # NVIDIA nodes
module spider rocm     # Kamand nodes
module spider openmpi  # See MPI versions and prerequisite compiler stack
```

## Lmod catalog by node family

All entries below are **module keys and versions** found by `module spider`; variants such as `-debug` and `-release` remain distinct. The list is grouped by physical node family and **does not double-count Slurm partition aliases**.

### Buran

**Surveyed hosts with usable module data:** 16. **Distinct module keys:** 43; **versioned entries:** 114.

| Module key | Versions exposed by `module spider` |
|---|---|
| `apache-spark` | `4.1.1` |
| `binutils` | `2.42`, `2.43`, `2.44` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.88.0-debug`, `1.88.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `dal` | `2025.6` |
| `debugger` | `2025.2.0` |
| `dev-utilities` | `2025.2.0` |
| `dnnl` | `3.8.1` |
| `dpct` | `2025.2.0` |
| `dpl` | `2022.9` |
| `gasnet` | `2023.9.0`, `2024.5.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `14.3.0`, `15.1.0`, `16.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2`, `1.14.6` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.11.2`, `2.12.0` |
| `intel` | `2024.2`, `2025.2` |
| `intel_ipp_intel64` | `2022.2` |
| `intel_ippcp_intel64` | `2025.2` |
| `ishmem` | `1.3.0` |
| `java` | `17` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `18.1.8`, `19.1.1`, `20.1.0`, `20.1.7`, `20.1.8`, `21.1.0`, `21.1.8`, `22.1.8` |
| `mkl` | `2025.2` |
| `mpi` | `2021.16` |
| `netcdf` | `4.9.2`, `4.9.3` |
| `netcdf-fortran` | `4.6.1`, `4.6.2` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.5`, `5.0.7` |
| `otf2` | `3.0.3` |
| `papi` | `7.1.0` |
| `pio` | `2.6.2`, `2.6.5` |
| `pmix` | `5.0.2`, `5.0.3`, `5.0.7` |
| `pnetcdf` | `1.13.0`, `1.14.0` |
| `python` | `3.12.3`, `3.13.2` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `tbb` | `2022.2` |
| `ucx` | `1.16.0`, `1.17.0`, `1.18.0` |
| `umf` | `0.11.0` |
| `zlib` | `1.3.1` |

### Medusa

**Surveyed hosts with usable module data:** 16. **Distinct module keys:** 43; **versioned entries:** 114.

| Module key | Versions exposed by `module spider` |
|---|---|
| `apache-spark` | `4.1.1` |
| `binutils` | `2.42`, `2.43`, `2.44` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.88.0-debug`, `1.88.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `dal` | `2025.6` |
| `debugger` | `2025.2.0` |
| `dev-utilities` | `2025.2.0` |
| `dnnl` | `3.8.1` |
| `dpct` | `2025.2.0` |
| `dpl` | `2022.9` |
| `gasnet` | `2023.9.0`, `2024.5.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `14.3.0`, `15.1.0`, `16.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2`, `1.14.6` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.11.2`, `2.12.0` |
| `intel` | `2024.2`, `2025.2` |
| `intel_ipp_intel64` | `2022.2` |
| `intel_ippcp_intel64` | `2025.2` |
| `ishmem` | `1.3.0` |
| `java` | `17` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `18.1.8`, `19.1.1`, `20.1.0`, `20.1.7`, `20.1.8`, `21.1.0`, `21.1.8`, `22.1.8` |
| `mkl` | `2025.2` |
| `mpi` | `2021.16` |
| `netcdf` | `4.9.2`, `4.9.3` |
| `netcdf-fortran` | `4.6.1`, `4.6.2` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.5`, `5.0.7` |
| `otf2` | `3.0.3` |
| `papi` | `7.1.0` |
| `pio` | `2.6.2`, `2.6.5` |
| `pmix` | `5.0.2`, `5.0.3`, `5.0.7` |
| `pnetcdf` | `1.13.0`, `1.14.0` |
| `python` | `3.12.3`, `3.13.2` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `tbb` | `2022.2` |
| `ucx` | `1.16.0`, `1.17.0`, `1.18.0` |
| `umf` | `0.11.0` |
| `zlib` | `1.3.1` |

### Marvin (15 of 16 nodes returned successful survey data; `marvin00` was unavailable)

**Surveyed hosts with usable module data:** 15. **Distinct module keys:** 43; **versioned entries:** 108.

**Host variation:** Family-wide union shown below; differences between responding hosts are listed after the table.

| Module key | Versions exposed by `module spider` |
|---|---|
| `apache-spark` | `4.1.1` |
| `binutils` | `2.42`, `2.43`, `2.44` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.88.0-debug`, `1.88.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `dal` | `2025.6` |
| `debugger` | `2025.2.0` |
| `dev-utilities` | `2025.2.0` |
| `dnnl` | `3.8.1` |
| `dpct` | `2025.2.0` |
| `dpl` | `2022.9` |
| `gasnet` | `2023.9.0`, `2024.5.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `14.3.0`, `15.1.0`, `16.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2`, `1.14.6` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.12.0` |
| `intel` | `2024.2`, `2025.2` |
| `intel_ipp_intel64` | `2022.2` |
| `intel_ippcp_intel64` | `2025.2` |
| `ishmem` | `1.3.0` |
| `java` | `17` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `18.1.8`, `19.1.1`, `20.1.0`, `20.1.8`, `21.1.8`, `22.1.8` |
| `mkl` | `2025.2` |
| `mpi` | `2021.16` |
| `netcdf` | `4.9.2`, `4.9.3` |
| `netcdf-fortran` | `4.6.1`, `4.6.2` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.7` |
| `otf2` | `3.0.3` |
| `papi` | `7.1.0` |
| `pio` | `2.6.2`, `2.6.5` |
| `pmix` | `5.0.2`, `5.0.7` |
| `pnetcdf` | `1.13.0`, `1.14.0` |
| `python` | `3.12.3`, `3.13.2` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `tbb` | `2022.2` |
| `ucx` | `1.16.0`, `1.18.0` |
| `umf` | `0.11.0` |
| `zlib` | `1.3.1` |

- `marvin04` does not advertise: `dal/2025.6`, `debugger/2025.2.0`, `dev-utilities/2025.2.0`, `dnnl/3.8.1`, `dpct/2025.2.0`, `dpl/2022.9`, `intel_ipp_intel64/2022.2`, `intel_ippcp_intel64/2025.2`, `ishmem/1.3.0`, `mkl/2025.2`, `mpi/2021.16`, `tbb/2022.2`, `umf/0.11.0`.

### Sven

**Surveyed hosts with usable module data:** 2. **Distinct module keys:** 14; **versioned entries:** 14.

| Module key | Versions exposed by `module spider` |
|---|---|
| `binutils` | `2.42` |
| `cmake` | `3.29.2` |
| `gcc` | `13.2.1` |
| `git` | `2.45.0` |
| `gperftools` | `2.15` |
| `hwloc` | `2.10.0` |
| `openmpi` | `5.0.3` |
| `otf2` | `3.0.3` |
| `pmix` | `5.0.2` |
| `python` | `3.12.3` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `ucx` | `1.16.0` |
| `zlib` | `1.3.1` |

### Spark

**Surveyed hosts with usable module data:** 4. **Distinct module keys:** 10; **versioned entries:** 10.

| Module key | Versions exposed by `module spider` |
|---|---|
| `binutils` | `2.44` |
| `boost` | `1.88.0-release` |
| `cmake` | `4.0.0` |
| `cuda` | `13.0` |
| `gcc` | `16.2.0` |
| `hwloc` | `2.12.0` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.10` |
| `pmix` | `5.0.7` |
| `ucx` | `1.19.0` |

### Anvil

**Surveyed hosts with usable module data:** 1. **Distinct module keys:** 42; **versioned entries:** 110.

| Module key | Versions exposed by `module spider` |
|---|---|
| `binutils` | `2.42`, `2.43`, `2.44` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.88.0-debug`, `1.88.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `cuda` | `12.8`, `12.9`, `13.4` |
| `dal` | `2025.6` |
| `debugger` | `2025.2.0` |
| `dev-utilities` | `2025.2.0` |
| `dnnl` | `3.8.1` |
| `dpct` | `2025.2.0` |
| `dpl` | `2022.9` |
| `gasnet` | `2023.9.0`, `2024.5.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `14.3.0`, `15.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2`, `1.14.6` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.11.2`, `2.12.0` |
| `intel` | `2025.2` |
| `intel_ipp_intel64` | `2022.2` |
| `intel_ippcp_intel64` | `2025.2` |
| `ishmem` | `1.3.0` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `18.1.8`, `19.1.1`, `20.1.0`, `20.1.8`, `21.1.8` |
| `mkl` | `2025.2` |
| `mpi` | `2021.16` |
| `netcdf` | `4.9.2`, `4.9.3` |
| `netcdf-fortran` | `4.6.1`, `4.6.2` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.5`, `5.0.7` |
| `otf2` | `3.0.3` |
| `papi` | `7.1.0` |
| `pio` | `2.6.2`, `2.6.5` |
| `pmix` | `5.0.2`, `5.0.3`, `5.0.7` |
| `pnetcdf` | `1.13.0`, `1.14.0` |
| `python` | `3.12.3`, `3.13.2` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `tbb` | `2022.2` |
| `ucx` | `1.16.0`, `1.17.0`, `1.18.0` |
| `umf` | `0.11.0` |
| `zlib` | `1.3.1` |

### Nasrin

**Surveyed hosts with usable module data:** 2. **Distinct module keys:** 42; **versioned entries:** 109.

| Module key | Versions exposed by `module spider` |
|---|---|
| `binutils` | `2.42`, `2.43`, `2.44` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.88.0-debug`, `1.88.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `cuda` | `12.8`, `12.9`, `13.4` |
| `dal` | `2025.6` |
| `debugger` | `2025.2.0` |
| `dev-utilities` | `2025.2.0` |
| `dnnl` | `3.8.1` |
| `dpct` | `2025.2.0` |
| `dpl` | `2022.9` |
| `gasnet` | `2023.9.0`, `2024.5.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `14.3.0`, `15.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2`, `1.14.6` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.11.2`, `2.12.0` |
| `intel` | `2025.2`, `tbb/2022.2` |
| `intel_ipp_intel64` | `2022.2` |
| `intel_ippcp_intel64` | `2025.2` |
| `ishmem` | `1.3.0` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `18.1.8`, `19.1.1`, `20.1.0`, `20.1.7`, `20.1.8`, `21.1.0`, `21.1.8` |
| `mkl` | `2025.2` |
| `mpi` | `2021.16` |
| `netcdf` | `4.9.3` |
| `netcdf-fortran` | `4.6.2` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.5`, `5.0.7` |
| `otf2` | `3.0.3` |
| `papi` | `7.1.0` |
| `pio` | `2.6.5` |
| `pmix` | `5.0.2`, `5.0.3`, `5.0.7` |
| `pnetcdf` | `1.14.0` |
| `python` | `3.12.3`, `3.13.2` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `tbb` | `2022.2` |
| `ucx` | `1.16.0`, `1.17.0`, `1.18.0` |
| `umf` | `0.11.0` |
| `zlib` | `1.3.1` |

### Toranj

**Surveyed hosts with usable module data:** 2. **Distinct module keys:** 42; **versioned entries:** 115.

| Module key | Versions exposed by `module spider` |
|---|---|
| `binutils` | `2.42`, `2.43`, `2.44` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.88.0-debug`, `1.88.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `cuda` | `12.8`, `12.9`, `13.4` |
| `dal` | `2025.6` |
| `debugger` | `2025.2.0` |
| `dev-utilities` | `2025.2.0` |
| `dnnl` | `3.8.1` |
| `dpct` | `2025.2.0` |
| `dpl` | `2022.9` |
| `gasnet` | `2023.9.0`, `2024.5.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `14.3.0`, `15.1.0`, `16.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2`, `1.14.6` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.11.2`, `2.12.0` |
| `intel` | `2025.2`, `tbb/2022.2` |
| `intel_ipp_intel64` | `2022.2` |
| `intel_ippcp_intel64` | `2025.2` |
| `ishmem` | `1.3.0` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `18.1.8`, `19.1.1`, `20.1.0`, `20.1.7`, `20.1.8`, `21.1.0`, `21.1.8`, `22.1.8` |
| `mkl` | `2025.2` |
| `mpi` | `2021.16` |
| `netcdf` | `4.9.2`, `4.9.3` |
| `netcdf-fortran` | `4.6.1`, `4.6.2` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.5`, `5.0.7` |
| `otf2` | `3.0.3` |
| `papi` | `7.1.0` |
| `pio` | `2.6.2`, `2.6.5` |
| `pmix` | `5.0.2`, `5.0.3`, `5.0.7` |
| `pnetcdf` | `1.13.0`, `1.14.0` |
| `python` | `3.12.3`, `3.13.2` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `tbb` | `2022.2` |
| `ucx` | `1.16.0`, `1.17.0`, `1.18.0` |
| `umf` | `0.11.0` |
| `zlib` | `1.3.1` |

### Diablo

**Surveyed hosts with usable module data:** 1. **Distinct module keys:** 42; **versioned entries:** 106.

| Module key | Versions exposed by `module spider` |
|---|---|
| `binutils` | `2.42`, `2.44` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.88.0-debug`, `1.88.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `cuda` | `12.2`, `12.4`, `12.8`, `12.9` |
| `dal` | `2025.6` |
| `debugger` | `2025.2.0` |
| `dev-utilities` | `2025.2.0` |
| `dnnl` | `3.8.1` |
| `dpct` | `2025.2.0` |
| `dpl` | `2022.9` |
| `gasnet` | `2023.9.0`, `2024.5.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `14.3.0`, `15.1.0`, `16.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2`, `1.14.6` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.12.0` |
| `intel` | `2025.2` |
| `intel_ipp_intel64` | `2022.2` |
| `intel_ippcp_intel64` | `2025.2` |
| `ishmem` | `1.3.0` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `19.1.1`, `20.1.0`, `21.1.8`, `22.1.8` |
| `mkl` | `2025.2` |
| `mpi` | `2021.16` |
| `netcdf` | `4.9.2`, `4.9.3` |
| `netcdf-fortran` | `4.6.1`, `4.6.2` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.7` |
| `otf2` | `3.0.3` |
| `papi` | `7.1.0` |
| `pio` | `2.6.2`, `2.6.5` |
| `pmix` | `5.0.2`, `5.0.7` |
| `pnetcdf` | `1.13.0`, `1.14.0` |
| `python` | `3.12.3`, `3.13.2` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `tbb` | `2022.2` |
| `ucx` | `1.16.0`, `1.18.0` |
| `umf` | `0.11.0` |
| `zlib` | `1.3.1` |

### Bahram

**Surveyed hosts with usable module data:** 1. **Distinct module keys:** 42; **versioned entries:** 112.

| Module key | Versions exposed by `module spider` |
|---|---|
| `binutils` | `2.42`, `2.43`, `2.44` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.88.0-debug`, `1.88.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `cuda` | `12.2`, `12.4`, `12.8`, `12.9` |
| `dal` | `2025.6` |
| `debugger` | `2025.2.0` |
| `dev-utilities` | `2025.2.0` |
| `dnnl` | `3.8.1` |
| `dpct` | `2025.2.0` |
| `dpl` | `2022.9` |
| `gasnet` | `2023.9.0`, `2024.5.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `14.3.0`, `15.1.0`, `16.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2`, `1.14.6` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.11.2`, `2.12.0` |
| `intel` | `2025.2` |
| `intel_ipp_intel64` | `2022.2` |
| `intel_ippcp_intel64` | `2025.2` |
| `ishmem` | `1.3.0` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `18.1.8`, `19.1.1`, `20.1.0`, `21.1.8`, `22.1.8` |
| `mkl` | `2025.2` |
| `mpi` | `2021.16` |
| `netcdf` | `4.9.2`, `4.9.3` |
| `netcdf-fortran` | `4.6.1`, `4.6.2` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.5`, `5.0.7` |
| `otf2` | `3.0.3` |
| `papi` | `7.1.0` |
| `pio` | `2.6.2`, `2.6.5` |
| `pmix` | `5.0.2`, `5.0.3`, `5.0.7` |
| `pnetcdf` | `1.13.0`, `1.14.0` |
| `python` | `3.12.3`, `3.13.2` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `tbb` | `2022.2` |
| `ucx` | `1.16.0`, `1.17.0`, `1.18.0` |
| `umf` | `0.11.0` |
| `zlib` | `1.3.1` |

### Kamand

**Surveyed hosts with usable module data:** 2. **Distinct module keys:** 42; **versioned entries:** 109.

| Module key | Versions exposed by `module spider` |
|---|---|
| `binutils` | `2.42`, `2.43`, `2.44` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `dal` | `2025.6` |
| `debugger` | `2025.2.0` |
| `dev-utilities` | `2025.2.0` |
| `dnnl` | `3.8.1` |
| `dpct` | `2025.2.0` |
| `dpl` | `2022.9` |
| `gasnet` | `2023.9.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `15.1.0`, `16.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.11.2`, `2.12.0` |
| `intel` | `2025.2` |
| `intel_ipp_intel64` | `2022.2` |
| `intel_ippcp_intel64` | `2025.2` |
| `ishmem` | `1.3.0` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `18.1.8`, `19.1.1`, `20.1.0`, `20.1.8`, `21.1.0`, `21.1.8`, `22.1.8` |
| `mkl` | `2025.2` |
| `mpi` | `2021.16` |
| `netcdf` | `4.9.2` |
| `netcdf-fortran` | `4.6.1` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.5` |
| `otf2` | `3.0.3` |
| `papi` | `7.1.0` |
| `pio` | `2.6.2` |
| `pmix` | `5.0.2`, `5.0.3` |
| `pnetcdf` | `1.13.0` |
| `python` | `3.12.3`, `3.13.2` |
| `rocm` | `5.4.6`, `5.5.1`, `5.6.1`, `5.7.3`, `6.0.3`, `6.1.3`, `6.2.4`, `6.3.2`, `6.4.1`, `7.14.1` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `tbb` | `2022.2` |
| `ucx` | `1.16.0`, `1.17.0`, `1.18.0` |
| `umf` | `0.11.0` |
| `zlib` | `1.3.1` |

### Rostam1 (login)

**Surveyed hosts with usable module data:** 1. **Distinct module keys:** 29; **versioned entries:** 104.

| Module key | Versions exposed by `module spider` |
|---|---|
| `apache-spark` | `4.1.1` |
| `binutils` | `2.42`, `2.43`, `2.44`, `2.45.1` |
| `boost` | `1.76.0-debug`, `1.76.0-release`, `1.77.0-debug`, `1.77.0-release`, `1.78.0-debug`, `1.78.0-release`, `1.79.0-debug`, `1.79.0-release`, `1.80.0-debug`, `1.80.0-release`, `1.81.0-debug`, `1.81.0-release`, `1.82.0-debug`, `1.82.0-release`, `1.83.0-debug`, `1.83.0-release`, `1.84.0-debug`, `1.84.0-release`, `1.85.0-debug`, `1.85.0-release`, `1.86.0-debug`, `1.86.0-release`, `1.87.0-debug`, `1.87.0-release`, `1.88.0-debug`, `1.88.0-release`, `1.89.0-debug`, `1.89.0-release`, `1.90.0-debug`, `1.90.0-release`, `1.91.0-debug`, `1.91.0-release`, `1.92.0-debug`, `1.92.0-release` |
| `cmake` | `3.29.2`, `4.0.0` |
| `gasnet` | `2023.9.0`, `2024.5.0` |
| `gcc` | `11.4.1`, `12.2.1`, `13.2.1`, `14.1.0`, `14.2.0`, `14.3.0`, `15.1.0`, `16.1.0`, `16.2.0` |
| `gdb` | `14.2`, `16.2` |
| `git` | `2.45.0`, `2.49.0` |
| `gperftools` | `2.15`, `2.16` |
| `hdf5` | `1.14.4.2`, `1.14.6` |
| `hub` | `2.14.2` |
| `hwloc` | `2.10.0`, `2.11.2`, `2.12.0` |
| `intel` | `2024.2`, `2025.2`, `2025.3` |
| `java` | `17` |
| `likwid` | `5.4.1` |
| `llvm` | `16.0.6`, `17.0.6`, `18.1.5`, `18.1.8`, `19.1.1`, `20.1.0`, `20.1.7`, `20.1.8`, `21.1.0`, `21.1.8`, `22.1.8` |
| `netcdf` | `4.9.2`, `4.9.3` |
| `netcdf-fortran` | `4.6.1`, `4.6.2` |
| `ninja` | `1.13.1` |
| `openmpi` | `5.0.3`, `5.0.5`, `5.0.7` |
| `papi` | `7.1.0` |
| `pio` | `2.6.2`, `2.6.5` |
| `pmix` | `5.0.2`, `5.0.3`, `5.0.7` |
| `pnetcdf` | `1.13.0`, `1.14.0` |
| `python` | `3.12.3`, `3.13.2` |
| `Rostam` | `2.4` |
| `szip` | `2.1.1` |
| `ucx` | `1.16.0`, `1.17.0`, `1.18.0` |
| `zlib` | `1.3.1` |

## NVIDIA HPC SDK: where it would fit (not installed)

NVIDIA HPC SDK is an **additional NVIDIA software distribution**, not a replacement for Slurm, the NVIDIA kernel driver, or the cluster's existing GCC/LLVM/Intel/CUDA module trees. No `nvhpc` installation was established by the October 9 survey. The following describes future deployment alternatives, **not currently available modules**.

| NVIDIA-provided module family | Compiler setup | MPI setup | GPU libraries / intended use |
|---|---|---|---|
| `nvhpc/<version>` | Adds `nvc`, `nvc++`, `nvfortran` | Exposes SDK-provided MPI | Integrated SDK compiler + CUDA/math/communication environment |
| `nvhpc-nompi/<version>` | Adds NVIDIA HPC compilers | **Does not** add SDK MPI | Use the NVIDIA compilers with an independently chosen, compatible MPI implementation |
| `nvhpc-byo-compilers/<version>` | **Does not** select NVIDIA HPC compilers | **Does not** add SDK MPI | Brings SDK CUDA/communication and other libraries while preserving the chosen external compiler and MPI toolchain |

Some releases also expose optional HPC-X-related module variants. Such variants depend on the particular SDK release and installation and should only be documented as available after inspection and testing.

**Placement and coexistence:** A versioned SDK installation could live under `/opt/nvidia/hpc_sdk`, with release-specific modulefiles made available through Lmod. New versions should remain separate from older versions. The SDK would initially be evaluated on applicable NVIDIA compute nodes; AMD MI100 nodes do not use NVIDIA CUDA, and CPU-only, RISC-V, and ARM64 targets require separate platform and vendor support checks. A GPU driver upgrade is not implied simply by publishing compiler modules. Compatibility with existing GCC, Open MPI, UCX, Slurm PMIx, driver levels, and V100/A100/GB10 target architectures must be validated before enabling any flavor.

**Programming models:** NVIDIA HPC compilers can offer C, C++, Fortran, OpenACC, and OpenMP offloading capabilities where supported. The `nvhpc-byo-compilers` variant **does not itself provide `nvc`, `nvc++`, or `nvfortran`**, and should not be presented as a way to use NVIDIA compiler-specific OpenACC support with arbitrary host compilers.

Reference: [NVIDIA HPC Compilers User Guide — Environment Modules](https://docs.nvidia.com/hpc-sdk/compilers/hpc-compilers-user-guide/#using-environment-modules-on-linux).

### Potential NVIDIA HPC SDK deployment choices on Rostam

The work for this is "in progress." We'll update this section as we progress.

| Future use case | Most relevant flavor | Compatibility consideration |
|---|---|---|
| Compile C/C++/Fortran with NVIDIA compilers and use packaged MPI | `nvhpc` | Check packaged MPI with Slurm, PMIx, interconnects and runtime requirements |
| Compile with `nvc`, `nvc++`, or `nvfortran` but retain a separately managed MPI | `nvhpc-nompi` | Validate compiler wrappers, MPI build/ABI compatibility and Slurm launch |
| Continue GCC/LLVM/Intel compilers and existing MPI while using selected SDK libraries | `nvhpc-byo-compilers` | Validate host compiler and CUDA-library compatibility; NVIDIA compilers are not enabled |

The three variants are **alternative module environments from an SDK installation**, not three separate hardware products. Each version can be installed alongside other versions, with one explicit flavor selected for a job or build. A change to the NVIDIA driver or to cluster-wide compiler defaults should not be assumed. Any eventual module policy should record which flavor, version, CUDA components and MPI combination were validated on each GPU family.

## Scope and maintenance

The survey queried Slurm compute nodes plus known infrastructure hosts. The catalog tables concentrate on compute nodes and the login host, since these are the software environments relevant to researchers. Storage and web infrastructure are not general-purpose compute environments and did not advertise a comparable Lmod catalog in this survey.

- `marvin00`, `jeremy`, and `marjorie` were not successfully surveyed in the deep scan. Their absence does not establish that software is missing.
- Version lists reflect the discovery results at the survey date, not current availability or a guarantee that a module loads.
- Module catalog entries do not establish binary compatibility, MPI launch behavior, or GPU toolkit/driver interoperability.
- To update this page, collect `module spider` on every node again, and separately validate dependencies with `module spider <name/version>`, `module show`, and test loads.

See also [Software Environment](/en/cluster/software), [Software Availability by System](/en/software/availability), [Hardware Resources](/en/cluster/hardware), and [Running GPU Jobs](/en/slurm/gpu).
