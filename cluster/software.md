---
title: Software Library
description: 
published: true
date: 2026-10-09T23:40:49.281Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:46.278Z
---


# Software Environment

Rostam is designed to provide a friendly and accessible computing environment for students, researchers, and developers who want to expand their knowledge of high-performance computing (HPC).

The cluster serves both newcomers learning parallel programming and experienced developers who need a flexible environment for testing, debugging, and validating software before moving to larger production computing systems.

Consistent with the principles of *Magnifica Humanitas*, Rostam emphasizes accessibility, exploration, collaboration, responsible use of shared computing resources, and reproducible scientific computing.

Rostam strives to incorporate modern software, compilers, programming languages, and development tools while maintaining conventional tools and established workflows. Its relatively small size and diverse hardware allow administrators to accommodate specialized research and development needs.

Because Rostam includes several processor architectures and operating systems, software availability is not necessarily identical across all nodes.

> **Looking for a particular compiler, library, or software package?**
>
> See [Software Availability by System](/en/software/availability) for a current summary of compilers, programming languages, MPI implementations, scientific libraries, and GPU development environments across Rostam's computing systems.

## Operating Systems

Rostam operates a heterogeneous collection of computing systems using several Linux distributions.

Historically, the cluster used CentOS. The main x86-64 compute environment has since transitioned to AlmaLinux 9, while specialized processor architectures and infrastructure systems use other Linux distributions.

As of the October 9, 2026 software inventory:

| Operating System | Systems | Architecture |
|---|---|---|
| AlmaLinux 9.6 | Most CPU and conventional GPU compute nodes | x86-64 |
| AlmaLinux 9.7 | Rostam login environment | x86-64 |
| Ubuntu 24.04 LTS | NVIDIA DGX Spark | ARM64 |
| Fedora Linux 38 | Sven | RISC-V |
| Red Hat Enterprise Linux 9.x | Selected infrastructure and storage systems | x86-64 |

The operating system provides the kernel, system compiler, runtime libraries, package management, and basic utilities.

For example, the main AlmaLinux 9.6 compute environment provides:

| Component | System Version |
|---|---|
| Operating System | AlmaLinux 9.6 |
| Linux Kernel | 5.14 series |
| GCC | 11.5.0 |
| glibc | 2.34 |
| GNU Binutils | 2.35 series |
| Python | 3.9 |
| CMake | 3.26.5 |
| LLVM/Clang | 19.1.7 |

These are system-provided versions. Additional compilers, language runtimes, development tools, and scientific libraries are available through environment modules.

The ARM64 and RISC-V systems maintain separate native software environments.

### Identifying Your System

To identify the operating system:

```bash
cat /etc/os-release
```

To check the processor architecture:

```bash
uname -m
```

To check the kernel:

```bash
uname -r
```

To display the hostname:

```bash
hostname
```

These commands are particularly useful when compiling software for different architectures.

## Software Organization

Software on Rostam is generally provided through three mechanisms.

### System Software

System software is installed through operating-system package repositories and managed by system administrators.

Examples include:

- Shells and basic command-line utilities
- Text editors
- System compilers
- Python and other language runtimes
- Standard development libraries
- Operating-system utilities

This software is generally available without loading environment modules.

For example:

```bash
gcc --version
python3 --version
cmake --version
```

The versions returned depend on the node and its operating system.

### Administrator-Managed Software

Rostam administrators maintain additional software installations to support research and development requirements.

Many of these installations are located under:

```text
/opt/apps
```

Software available through this environment includes:

- Additional GCC compiler versions
- LLVM/Clang
- Boost C++ libraries
- Python interpreters
- CMake
- Open MPI
- UCX and PMIx
- Scientific data libraries
- GPU development toolkits

These installations are commonly made available through Lmod environment modules.

Users generally do not need administrative privileges to access module-managed software.

### Compiler-Dependent Software

Some libraries must be compiled for a particular compiler, standard library, or dependency environment.

For example, a Boost library compiled using GCC may not be interchangeable with one compiled using Clang.

Similarly, MPI libraries and applications must use compatible compiler and runtime environments.

Rostam uses a hierarchical module configuration to organize such dependencies.

Some modules become available only after loading the appropriate compiler or prerequisite software.

This helps prevent users from accidentally combining incompatible tools or libraries.

## Compiler Compatibility and ABIs

Compiler compatibility is particularly important in high-performance and scientific computing.

Applications frequently link against C, C++, Fortran, MPI, and numerical libraries.

These components must use compatible application binary interfaces (ABIs), runtime libraries, and calling conventions.

For example, GCC 5 introduced changes to the C++ standard library ABI affecting certain types, including `std::string` and `std::list`.

Mixing libraries compiled with incompatible ABI settings can result in linking failures or runtime problems.

Modern Rostam systems avoid many of the limitations associated with older CentOS 7 installations and GCC 4.8, but compiler compatibility remains important.

### Recommended Practices

- Use a consistent compiler when building applications and their dependencies.
- Load the desired compiler before building compiler-dependent libraries.
- Avoid mixing incompatible MPI implementations.
- Record compiler and library versions for important builds.
- Rebuild software when switching between incompatible compiler environments.
- Test applications when upgrading compilers or runtime libraries.

The newest compiler is not necessarily the best choice for every application.

Some scientific software requires a particular compiler version for compatibility or reproducibility.

Rostam maintains multiple compiler versions to accommodate these requirements.

## Environment Modules

Rostam uses [Lmod](https://lmod.readthedocs.io/), a Lua-based environment module system developed at the Texas Advanced Computing Center (TACC).

Lmod allows users to select software packages and versions without modifying system-wide installations.

An environment module changes shell environment settings so that the corresponding applications, libraries, headers, and related resources become accessible.

For example, loading a module may modify:

- `PATH`
- `LD_LIBRARY_PATH`
- `CPATH`
- `LIBRARY_PATH`
- `PKG_CONFIG_PATH`
- `MODULEPATH`

Loading a module does not install software. It configures your environment to use an existing installation.

### Listing Available Modules

To list modules currently available:

```bash
module avail
```

This lists modules accessible within the current environment.

Because Rostam uses hierarchical module paths, the available modules may change when different compilers or prerequisite libraries are loaded.

### Searching for Software

Use `module spider` to search the complete module hierarchy:

```bash
module spider
```

To search for a particular package:

```bash
module spider boost
```

To inspect a specific version:

```bash
module spider boost/1.92.0-release
```

The detailed output may explain which compiler or prerequisite modules are required before the package can be loaded.

### Loading Modules

To load a particular GCC version:

```bash
module load gcc/16.2.0
```

Verify the active compiler:

```bash
gcc --version
```

To load the default version of a module:

```bash
module load gcc
```

The default version may change as the software environment is maintained.

For reproducible builds, specifying the exact module version is preferable.

### Loading Multiple Modules

Modules can be combined when their software environments are compatible.

For example:

```bash
module load gcc/16.2.0
module load cmake
```

Additional scientific libraries can then be loaded as required.

Before loading compiler-dependent libraries, check their prerequisites using `module spider`.

### Viewing Loaded Modules

To list currently loaded modules:

```bash
module list
```

This is useful when checking which compiler, runtime, and libraries are active.

### Unloading Modules

To unload a module:

```bash
module unload cmake
```

To unload a compiler:

```bash
module unload gcc
```

Lmod ordinarily reverses the environment changes introduced by the module.

### Switching Compilers

Lmod understands relationships and conflicts between software environments.

Loading a different compiler may automatically replace the currently loaded compiler and adjust dependent modules.

For example:

```bash
module load gcc/16.2.0
```

To explicitly replace GCC with another installed version:

```bash
module swap gcc/16.2.0 gcc/14.2.1
```

After switching:

```bash
module list
gcc --version
```

Some dependent modules may be reloaded automatically, while others may become unavailable.

Switching compiler modules does not recompile software already built with another compiler.

### Resetting the Environment

To unload all modules:

```bash
module purge
```

To restore the system-defined default environment:

```bash
module reset
```

The default environment may contain no loaded modules.

Users should not assume that resetting the module environment automatically loads a compiler or predefined Rostam software collection.

Inspect the resulting environment:

```bash
module list
```

### Saving Module Collections

Lmod allows users to save collections of modules for later use.

For example:

```bash
module load gcc/16.2.0
module load cmake
```

Save the current collection:

```bash
module save research-gcc16
```

List saved collections:

```bash
module savelist
```

Restore a collection:

```bash
module restore research-gcc16
```

Saved collections are useful for projects requiring different compiler or dependency environments.

A collection references existing modules. If those modules are changed or removed, the collection may need to be updated.

### Module Help

Display information about a particular module:

```bash
module help gcc
```

Display Lmod help:

```bash
module help
```

### Module Paths

Lmod searches directories listed in the `MODULEPATH` environment variable.

Inspect the current search path:

```bash
echo "$MODULEPATH"
```

On the main x86-64 compute environment, module directories include:

```text
/opt/apps/modulefiles
```

Additional module paths include system directories and Intel oneAPI modulefiles.

ARM64 and RISC-V systems have architecture-specific software environments.

For an overview of software available by system, see [Software Availability by System](/en/software/availability).

## Python

Python is available across Rostam, although the default version varies by operating system and architecture.

The main AlmaLinux 9.6 compute systems provide Python 3.9 as their system interpreter.

Additional Python versions, including 3.12 and 3.13, are available in the administrator-managed x86-64 software environment.

The DGX Spark systems provide Python 3.12, while the RISC-V systems use Python 3.11 as their system default.

### Checking Python

Check the active Python version:

```bash
python3 --version
```

Identify the executable:

```bash
which python3
```

### Selecting a Python Version

To discover Python modules:

```bash
module spider python
```

For example, on the main x86-64 environment:

```bash
module load python/3.12.3
```

Verify the active interpreter:

```bash
python3 --version
```

### Python Virtual Environments

Python virtual environments are recommended for managing project-specific libraries.

A virtual environment allows users to install packages without modifying the operating-system Python environment or interfering with other projects.

First, select the appropriate Python interpreter.

Create a virtual environment:

```bash
python3 -m venv ~/venvs/myproject
```

Activate it:

```bash
source ~/venvs/myproject/bin/activate
```

Install packages:

```bash
python -m pip install numpy
```

Or:

```bash
python -m pip install numpy scipy matplotlib
```

To deactivate:

```bash
deactivate
```

The environment remains available for subsequent sessions.

### Managing Dependencies

Record Python dependencies:

```bash
python -m pip freeze > requirements.txt
```

Reinstall them in another compatible environment:

```bash
python -m pip install -r requirements.txt
```

Some Python packages include architecture-specific compiled libraries.

A virtual environment created on x86-64 should not be assumed to work unchanged on ARM64 or RISC-V.

### User Package Installations

Python can also support packages installed in a user's home directory:

```bash
python3 -m pip install --user package-name
```

However, user installations can conflict with module-managed Python environments.

Virtual environments are preferred for project-specific dependencies.

Avoid using `sudo pip` to modify the shared operating-system Python installation.

## MPI and Parallel Computing

Rostam supports distributed and parallel computing through MPI and supporting communication libraries.

The main x86-64 environment provides multiple Open MPI versions and related software, including:

- UCX
- PMIx
- hwloc
- GASNet
- Intel MPI components

These libraries support message passing, process management, hardware topology discovery, and high-performance communication.

### Discovering MPI

Search for available MPI modules:

```bash
module spider openmpi
```

Inspect the desired version and its compiler dependencies before loading it.

### Checking MPI

After loading an appropriate MPI module:

```bash
mpicc --version
```

For Open MPI, the compiler wrapper configuration can be inspected using:

```bash
mpicc --showme
```

The selected MPI implementation should use the intended compiler and compatible runtime dependencies.

### MPI and Slurm

Slurm is responsible for allocating resources and launching computational jobs.

MPI applications must use a compatible process management configuration.

Applications built against one MPI implementation should not be assumed compatible with another.

The required job resources should be requested through Slurm before launching computational workloads.

## Scientific Libraries

The main x86-64 environment provides numerous scientific and development libraries.

| Category | Examples |
|---|---|
| C++ Libraries | Boost |
| Scientific Data | HDF5, NetCDF, NetCDF-Fortran |
| Parallel Data | PnetCDF, PIO |
| Parallel Computing | Open MPI, UCX, PMIx, GASNet |
| Numerical Computing | Intel oneMKL and other installed numerical libraries |
| Performance Analysis | PAPI, LIKWID, OTF2 |
| Build Tools | CMake, GNU Make, Ninja |
| Containers | Apptainer |

Different versions may be available depending on the selected compiler and module environment.

Use `module spider` to discover versions and prerequisites.

See [Software Availability by System](/en/software/availability) for the current per-system summary.

## GPU Software Environments

Rostam provides both NVIDIA and AMD GPU computing resources.

| Accelerator | Primary Software Ecosystem |
|---|---|
| NVIDIA V100 | CUDA |
| NVIDIA A100 | CUDA |
| NVIDIA GB10 | CUDA |
| AMD MI100 | ROCm, HIP |

GPU software compatibility depends on the hardware architecture, device drivers, toolkit versions, compilers, and runtime libraries.

### NVIDIA CUDA

CUDA provides development and runtime support for NVIDIA GPU applications.

To discover available CUDA modules:

```bash
module spider cuda
```

After loading a compatible toolkit, inspect the CUDA compiler:

```bash
nvcc --version
```

To inspect the NVIDIA driver and installed GPU:

```bash
nvidia-smi
```

The CUDA version reported by `nvidia-smi` indicates the CUDA API compatibility level supported by the installed driver.

It does not necessarily identify the version of the CUDA toolkit currently selected through Lmod.

### AMD ROCm

ROCm provides development and runtime support for AMD GPUs.

On Kamand, GPU information can be inspected using:

```bash
rocm-smi
```

Search for available ROCm modules:

```bash
module spider rocm
```

ROCm and HIP application compatibility depends on the supported accelerator architecture and installed software environment.

### GPU Compiler Compatibility

GPU development toolkits may support only specific host compiler versions.

Users should verify:

- The installed GPU driver.
- The selected CUDA or ROCm toolkit.
- The required host compiler.
- GPU architecture compatibility.
- Runtime and scientific library dependencies.

Loading a newer GCC module does not guarantee compatibility with a particular CUDA or ROCm release.

See [Rostam Hardware](/en/cluster/hardware) for hardware and accelerator information.

## Containers

Apptainer is available on the main x86-64 compute environment.

Apptainer enables users to run applications within containerized software environments while integrating with the host operating system and HPC infrastructure.

To check the installed version:

```bash
apptainer --version
```

Containers can improve portability and reproducibility, but they do not eliminate architecture compatibility requirements.

For example, an x86-64 container cannot ordinarily execute natively on ARM64 or RISC-V hardware.

GPU-enabled containers must also be compatible with the host's GPU driver and runtime environment.

## Reproducible Software Environments

Rostam encourages users to record the software environment associated with computational experiments and important research results.

Relevant information includes:

- Operating system and architecture.
- Compiler version.
- Loaded environment modules.
- MPI implementation.
- Numerical libraries.
- Python dependencies.
- CUDA or ROCm versions.
- Slurm job configuration.

### Recording Modules

List the current modules:

```bash
module list
```

Save the module list to a file:

```bash
module list 2> modules-used.txt
```

### Recording Compiler Versions

```bash
gcc --version
g++ --version
python3 --version
```

### Recording System Information

```bash
hostname
uname -m
cat /etc/os-release
```

### Recording Slurm Information

Within an allocated job:

```bash
scontrol show job "$SLURM_JOB_ID"
```

This information can help reproduce results, investigate application failures, and understand differences between computing environments.

## Software Availability

The Rostam software environment evolves as new compilers, libraries, operating systems, and research requirements are introduced.

Installed software and module availability may differ between node families and processor architectures.

For a concise inventory of current software, see:

**[Software Availability by System](/en/software/availability)**

The availability page summarizes:

- Operating systems and architectures.
- System and module-managed compilers.
- Python versions.
- MPI implementations.
- Scientific libraries.
- NVIDIA CUDA and AMD ROCm environments.
- Architecture-specific software.

For the most current module information, use `module spider` on the system where the application will run.

## Additional Documentation

- [Software Availability by System](/en/software/availability)
- [Rostam Hardware Inventory](/en/cluster/hardware)
- [Accessing Rostam](/en/ssh)
- [Advanced SSH Configuration](/en/ssh/advanced_ssh)
- [Lmod Documentation](https://lmod.readthedocs.io/)
- [Slurm Workload Manager](https://slurm.schedmd.com/documentation.html)
- [NVIDIA CUDA Documentation](https://docs.nvidia.com/cuda/)
- [AMD ROCm Documentation](https://rocm.docs.amd.com/)

---

*Software environment information last reviewed against the Rostam software survey of October 9, 2026.*
