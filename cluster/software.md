---
title: Software Library
description: 
published: true
date: 2026-10-09T23:50:36.102Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:46.278Z
---

# Software Environment

Rostam is designed as an approachable environment for students learning high-performance computing and a flexible testbed for experienced researchers and developers. Its diverse systems support experimentation, portability testing, and preparation for larger production systems. In keeping with *Magnifica Humanitas*, users are encouraged to share computing resources responsibly and document their work reproducibly.

**Looking for a particular package or compiler?** See [Software Availability by System](/en/software/availability), which summarizes installed software by node family. See [Hardware Resources](/en/cluster/hardware) for processor and GPU details.

## Operating systems

Rostam no longer uses the CentOS 8 environment described in older documentation. The October 9, 2026 inventory found these principal operating systems:

| Platform | Operating system | Architecture |
|---|---|---|
| Most CPU and conventional GPU compute nodes | AlmaLinux 9.6 | x86-64 |
| Login environment | AlmaLinux 9.7 | x86-64 |
| DGX Spark | Ubuntu 24.04 LTS | ARM64 |
| Sven | Fedora 38 | RISC-V |
| Selected storage and infrastructure | RHEL 9.x | x86-64 |

Check the machine you are on:

```bash
hostname
cat /etc/os-release
uname -m
uname -r
```

The main AlmaLinux 9.6 compute nodes have GCC 11.5.0, glibc 2.34, and Python 3.9 as their system defaults. Additional versions are installed separately. Never assume that every node has identical software.

## How software is provided

**System packages** come from operating-system repositories and are usually available without loading modules. Examples include shells, basic tools, a system compiler, and Python.

**Administrator-managed applications**, often under `/opt/apps`, provide newer compilers, MPI implementations, scientific libraries, and other packages without changing system defaults.

**Compiler- and architecture-dependent software** may require a particular GCC, Clang, MPI, CUDA, ROCm, or supporting library environment. A package built for x86-64 will not ordinarily execute on ARM64 or RISC-V without rebuilding.

## Compilers and ABI compatibility

GCC and C++ libraries need compatible application binary interfaces (ABIs). Libraries built using different compilers, C++ ABI settings, or MPI implementations are not necessarily interchangeable. GCC 5 introduced a change affecting selected C++ standard-library types; modern environments avoid the original CentOS 7/GCC 4.8 problem, but ABI compatibility still matters.

For reproducible builds, explicitly select compiler versions, build dependent libraries against the same compatible toolchain, and record the modules used. Newer is not always better: some applications require a tested older compiler.

## Environment modules (Lmod)

Rostam uses [Lmod](https://lmod.readthedocs.io/) to select available software. Modules adjust environment variables, such as `PATH`, library paths, and `MODULEPATH`; they do not install packages.

```bash
module avail                  # modules visible in the current environment
module spider                 # search the full module hierarchy
module spider boost           # find Boost versions
module spider gcc/16.2.0      # inspect a specific module
module load gcc/16.2.0        # load an explicit compiler version
module list                   # show loaded modules
module unload gcc             # unload a module
module purge                  # unload all modules
module reset                  # restore site defaults
```

Some modulefiles are hidden until a compatible compiler or prerequisite module is loaded. Use `module spider <name>/<version>` to learn the required sequence. Switching compilers may unload or reload dependent modules; it does **not** rebuild your application.

You can save a named collection:

```bash
module load gcc/16.2.0
module save research-gcc16
module savelist
module restore research-gcc16
```

Collections depend on the continued availability of the underlying module versions. The site default may load no modules; do **not** assume that `module reset` loads an old `Rostam2` module.

Show the current module search path:

```bash
echo "$MODULEPATH" | tr ':' '
'
```

The main x86-64 environment includes `/opt/apps/modulefiles`; module paths on ARM64 and RISC-V differ.

## Python and packages

The system Python varies by platform (typically 3.9 on the main AlmaLinux nodes, 3.11 on Sven, and 3.12 on Spark). Additional interpreters are available in some module environments.

```bash
python3 --version
command -v python3
module spider python
```

Prefer a project-local virtual environment over installing into system Python:

```bash
python3 -m venv ~/venvs/myproject
source ~/venvs/myproject/bin/activate
python -m pip install numpy scipy
python -m pip freeze > requirements.txt
deactivate
```

Recreate packages with `python -m pip install -r requirements.txt` in a compatible environment. Wheels containing compiled code may differ by processor architecture. Avoid `sudo pip` and do not assume `pip` always defaults to `--user`.

## Parallel computing and MPI

MPI implementations and support libraries such as Open MPI, Intel MPI, UCX, PMIx, and hwloc are available on selected systems. MPI and application binaries should use compatible compiler and library versions.

```bash
module spider openmpi
module list
mpicc --showme       # for Open MPI, after loading a compatible module
srun --mpi=list      # inspect supported Slurm MPI launch types
```

Slurm allocates CPU and GPU resources and launches jobs. Use an MPI launch mode that matches the environment; on Rostam, PMIx has been a useful choice for supported workloads. Test small jobs before scaling.

## GPU software

| Accelerator | Primary programming ecosystem |
|---|---|
| NVIDIA V100, A100, GB10 | CUDA |
| AMD MI100 | ROCm / HIP |

```bash
module spider cuda
module spider rocm
nvidia-smi           # NVIDIA nodes
rocm-smi             # AMD nodes
```

`nvidia-smi` reports the installed driver's maximum advertised CUDA API level, not necessarily the selected `nvcc` toolkit version. CUDA/ROCm toolchains must support the host compiler and accelerator. **NVIDIA HPC SDK is not documented as installed on Rostam.**

For GPU job submission and resource requests, see [Running GPU Jobs](/en/slurm/gpu).

## Scientific libraries and containers

Rostam's main x86-64 collection includes software such as Boost, HDF5, NetCDF, CMake, Open MPI, PMIx, UCX, and Apptainer. Availability differs by node family and compiler selection; consult the [Software Availability table](/en/software/availability) and `module spider`.

Containers can improve reproducibility but do not eliminate CPU architecture and host GPU driver requirements. Check `apptainer --version` on the intended node before assuming it is installed.

## Recording your environment

Keep a record with your important results or Slurm job output:

```bash
hostname
uname -m
cat /etc/os-release
module list 2>&1
gcc --version
python3 --version
scontrol show job "$SLURM_JOB_ID"    # within a Slurm job
```

Record module versions explicitly in batch scripts. A saved module collection or dependency file helps reproduce a result but may need updating after a cluster upgrade.

## Related documentation

- [Software Availability by System](/en/software/availability)
- [Hardware Resources](/en/cluster/hardware)
- [Running GPU Jobs](/en/slurm/gpu)
- [Connecting to Rostam](/en/ssh)
- [Advanced SSH Configuration](/en/ssh/advanced_ssh)

*Software inventory baseline: October 9, 2026.*
