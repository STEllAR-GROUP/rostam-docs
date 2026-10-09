---
title: Software Library
description: 
published: true
date: 2026-10-09T23:35:51.937Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:46.278Z
---


# Software Environment

Rostam is designed to provide a friendly and accessible computing environment for students, researchers, and developers who want to expand their knowledge of high-performance computing (HPC).

The cluster supports both newcomers learning parallel programming and experienced developers who need a flexible environment for testing, debugging, and validating software before moving to larger production computing systems.

Consistent with the principles of *Magnifica Humanitas*, Rostam emphasizes accessibility, exploration, responsible use of shared resources, and reproducible scientific computing.

Rostam strives to incorporate modern software, programming languages, compilers, and development tools while maintaining compatibility with conventional computing environments. Its relatively small size and diverse hardware allow administrators to accommodate specialized research and development needs.

Because Rostam contains several processor architectures and operating systems, software availability is not necessarily identical on every node.

> **Looking for a particular compiler, library, or development environment?**
>
> See [Software Availability by System](/software/availability) for a summary of installed software, compiler versions, scientific libraries, and GPU development environments across Rostam's computing platforms.

## Operating Systems

Rostam operates a heterogeneous collection of systems using several Linux distributions.

Historically, the cluster used CentOS. The computing environment has since transitioned primarily to AlmaLinux, with additional operating systems supporting specialized architectures and infrastructure.

As of the October 9, 2026 software inventory, the principal operating systems are:

| Operating System | Systems | Architecture |
|---|---|---|
| AlmaLinux 9.6 | Most CPU and conventional GPU compute nodes | x86-64 |
| AlmaLinux 9.7 | Rostam login environment | x86-64 |
| Ubuntu 24.04 LTS | NVIDIA DGX Spark | ARM64 |
| Fedora Linux 38 | Sven | RISC-V |
| Red Hat Enterprise Linux 9.x | Selected infrastructure and storage systems | x86-64 |

The operating system provides the system compiler, runtime libraries, kernel, and basic utilities.

For example, most AlmaLinux 9.6 compute nodes provide:

| Component | System Version |
|---|---|
| Operating system | AlmaLinux 9.6 |
| Linux kernel | 5.14 series |
| GCC | 11.5.0 |
| glibc | 2.34 |
| GNU binutils | 2.35 series |
| Python | 3.9.21 |
| CMake | 3.26.5 |
| LLVM/Clang | 19.1.7 |

These are system-provided versions. Newer compilers, Python interpreters, build tools, and scientific libraries are available through the environment module system.

The ARM64 and RISC-V platforms have their own native software environments.

### Why Different Operating Systems?

Different processor architectures require different operating-system support, compiler toolchains, and software packages.

Maintaining more than one Linux environment allows Rostam to support conventional x86-64 computing alongside emerging architectures such as ARM64 and RISC-V.

It also allows researchers to test whether software can be compiled and executed across different environments.

Applications may require recompilation or different dependencies when moving between architectures.

To identify the operating system on your current node:

```bash
cat /etc/os-release
```

To identify its processor architecture:

```bash
uname -m
```

To check the running kernel:

```bash
uname -r
```

## Software Organization

Software on Rostam is generally provided through three mechanisms.

### System Software

System software is installed using the operating system's package management tools.

Examples include:

- Shell utilities and text editors.
- System compilers.
- Python and other language runtimes.
- Basic development libraries.
- Operating-system utilities.

These applications are generally available without loading an environment module.

For example:

```bash
gcc --version
python3 --version
cmake --version
```

The exact versions depend on the node and operating system.

### Administrator-Managed Software

Rostam administrators maintain additional software installations, primarily through shared or architecture-specific application directories.

Many of these installations are located under:

```text
/opt/apps
```

Examples include:

- Additional GCC versions.
- LLVM/Clang toolchains.
- Boost C++ libraries.
- Python installations.
- CMake releases.
- Open MPI and supporting libraries.
- Scientific data libraries.
- GPU development environments.

This software is commonly accessed through Lmod environment modules.

### Compiler-Dependent Software

Certain scientific libraries must be compiled with specific compilers, compiler flags, or dependency versions.

For example, a Boost installation built using GCC may not be interchangeable with one built using Clang.

Similarly, MPI libraries and their dependent applications must use compatible compiler and runtime environments.

Rostam uses hierarchical environment modules to organize these dependencies.

Some modules may become available only after loading the appropriate compiler or prerequisite environment.

This reduces accidental combinations of incompatible software.

For a current overview, see [Software Availability by System](/software/availability).

## Compiler Compatibility and Application Binary Interfaces

Compiler compatibility is particularly important in scientific computing.

Applications are frequently linked against external C, C++, Fortran, MPI, or numerical libraries.

These components must use compatible application binary interfaces (ABIs), runtime libraries, and calling conventions.

For example, GCC 5 introduced a new C++ standard library ABI affecting certain types, including `std::string` and `std::list`.

Libraries built using incompatible ABI settings can produce linker errors or runtime problems.

Modern Rostam systems use newer operating systems and compilers, avoiding many of the problems associated with older CentOS 7 environments and GCC 4.8.

However, compiler compatibility remains important when mixing libraries or upgrading software.

### Recommended Practices

- Use a consistent compiler environment when building an application and its dependencies.
- Load the appropriate compiler module before compiling dependent libraries.
- Avoid mixing incompatible MPI implementations.
- Record compiler and library versions used for important builds.
- Rebuild applications when switching between incompatible compiler or ABI environments.
- Test applications after changing compiler or runtime versions.

The newest compiler is not necessarily the best choice for every application. Some software may require an older, better-tested compiler or a particular ABI.

Rostam maintains multiple compiler versions to accommodate these requirements.

## Environment Modules

Rostam uses [Lmod](https://lmod.readthedocs.io/), a Lua-based environment module system developed at the Texas Advanced Computing Center (TACC).

Lmod allows users to select software packages and versions without modifying system-wide installations.

An environment module changes shell settings so that executables, libraries, headers, and other resources become available to the user.

For example, a module may modify:

```bash
PATH
LD_LIBRARY_PATH
CPATH
LIBRARY_PATH
PKG_CONFIG_PATH
```

Loading a module does not install software. It configures your environment to use an existing installation.

### Listing Available Modules

To list software modules currently available:

```bash
module avail
```

This displays modules accessible within your current environment.

Rostam uses hierarchical module paths, so the output may change depending on which compiler or other prerequisites are loaded.

### Searching for Software

Use `module spider` to search the complete Lmod module hierarchy:

```bash
module spider
```

To search for a particular package:

```bash
module spider boost
```

To inspect a particular version:

```bash
module spider boost/1.92.0-release
```

The detailed output may explain which compiler or prerequisite modules must be loaded before the package becomes available.

### Loading a Module

To load a software package:

```bash
module load gcc/16.2.0
```

Verify the active compiler:

```bash
gcc --version
```

To load a package using its configured default version:

```bash
module load gcc
```

The default version may change as the software environment is maintained.

For reproducible builds, explicitly specifying the version is recommended.

### Loading Multiple Modules

For example:

```bash
module load gcc/16.2.0
module load cmake
```

Additional scientific libraries can be loaded as needed.

Before using a compiler-dependent library, check its prerequisites using `module spider`.

### Viewing Loaded Modules

To display the modules currently loaded:

```bash
module list
```

This helps identify the exact software environment used for building or running an application.

### Unloading Modules

To unload an individual module:

```bash
module unload cmake
```

To unload the GCC module:

```bash
module unload gcc
```

Lmod normally reverses the environment changes introduced by the unloaded module.

### Switching Compilers

Lmod understands conflicts between software environments.

For example, loading a different compiler may cause Lmod to replace the currently loaded compiler.

Dependent modules may be reloaded, unloaded, or reported as incompatible.

To explicitly replace one compiler with another:

```bash
module swap gcc/16.2.0 gcc/14.2.1
```

The requested versions must exist and be accessible in the current module hierarchy.

Always inspect the result:

```bash
module list
gcc --version
```

Compiler switching does not automatically rebuild previously compiled applications or libraries.

### Resetting the Environment

To unload all modules:

```bash
module purge
```

To restore the site's default module environment:

```bash
module reset
```

The default environment may contain no loaded modules.

Do not assume that `module reset` automatically loads a particular compiler, application collection, or legacy Rostam configuration.

Inspect the current state using:

```bash
module list
```

### Saving a Module Collection

Lmod allows users to save a collection of loaded modules for later use.

First, load the software environment you want:

```bash
module load gcc/16.2.0
module load cmake
```

Save it under a descriptive name:

```bash
module save research-gcc16
```

List saved collections:

```bash
module savelist
```

Restore the collection later:

```bash
module restore research-gcc16
```

Named module collections are useful when working on multiple projects with different software requirements.

A saved collection refers to module versions and configurations that must remain available. If modules are removed or changed, an old collection may need to be updated.

### Module Help

To display help for a particular module:

```bash
module help gcc
```

To display Lmod help:

```bash
module help
```

### Module Paths

The environment variable `MODULEPATH` determines which module directories Lmod searches.

To display it:

```bash
echo "$MODULEPATH"
```

On the main x86-64 compute systems, module paths include:

```text
/opt/apps/modulefiles
```

Additional paths include system module directories and Intel oneAPI modulefiles.

The ARM64 and RISC-V environments have architecture-specific differences.

Users generally should not need to manually edit `MODULEPATH` to access the administrator-managed software collection.

## Python Environments

Python is available throughout Rostam, but the default version depends on the operating system and architecture.

The main AlmaLinux 9.6 compute systems provide Python 3.9 as their default system interpreter.

Additional Python releases, including Python 3.12 and 3.13, are available through the module system on the main x86-64 environment.

The DGX Spark systems use Python 3.12, while the RISC-V systems use Python 3.11 as their system default.

### Checking Python

To check the active Python version:

```bash
python3 --version
```

To identify the interpreter being used:

```bash
which python3
```

### Loading Another Python Version

Search for available Python modules:

```bash
module spider python
```

On the main x86-64 module environment, for example:

```bash
module load python/3.12.3
```

Then verify:

```bash
python3 --version
```

### Python Virtual Environments

Python virtual environments are recommended for managing project-specific packages.

A virtual environment allows users to install libraries without modifying the operating system's Python environment or interfering with other projects.

First, choose the appropriate Python interpreter or load a Python module.

Create a virtual environment:

```bash
python3 -m venv ~/venvs/myproject
```

Activate it:

```bash
source ~/venvs/myproject/bin/activate
```

Check the interpreter:

```bash
python --version
```

Install packages:

```bash
python -m pip install numpy
```

Install multiple packages:

```bash
python -m pip install numpy scipy matplotlib
```

To deactivate:

```bash
deactivate
```

The installed packages remain in the virtual environment and can be used again when it is reactivated.

### Recording Python Dependencies

To record the installed Python packages:

```bash
python -m pip freeze > requirements.txt
```

To recreate the environment later:

```bash
python -m pip install -r requirements.txt
```

Some scientific packages include compiled native libraries and may depend on the system architecture or compiler environment.

An environment created on an x86-64 node should not be assumed to work unchanged on ARM64 or RISC-V.

### Installing Packages in Your Home Directory

When virtual environments are not appropriate, Python may support per-user package installations:

```bash
python3 -m pip install --user package-name
```

However, user installations can conflict with module-managed Python versions or other environments.

**Virtual environments are preferred for project-specific dependencies.**

Do not use `sudo pip` to install Python packages into the shared operating-system environment.

## Parallel and Distributed Computing Libraries

Rostam supports parallel computing through MPI and other communication libraries.

The main x86-64 software collection includes multiple versions of Open MPI, together with supporting libraries such as:

- UCX
- PMIx
- hwloc
- GASNet
- Intel MPI components

These provide communication, process management, and hardware topology support for distributed applications.

### Finding MPI Modules

Search for MPI implementations:

```bash
module spider openmpi
```

Inspect the requirements for the desired version before loading it.

### Checking MPI

After loading a suitable MPI environment:

```bash
mpicc --version
```

To identify the MPI wrapper configuration, where supported:

```bash
mpicc --showme
```

The MPI compiler wrapper should use the intended compiler and compatible dependencies.

### MPI and Slurm

Slurm manages job scheduling and process launch.

For an MPI application, users should request the necessary resources through Slurm and use the appropriate process-launch method.

The MPI implementation and Slurm's configured process management interface must be compatible.

Applications built using one MPI implementation should not be assumed compatible with another.

## Scientific Libraries

The main x86-64 software environment includes a range of scientific and numerical libraries.

Examples include:

| Category | Software |
|---|---|
| C++ libraries | Boost |
| Scientific data | HDF5, NetCDF, PnetCDF |
| Parallel I/O | Parallel NetCDF, PIO |
| Parallel communication | Open MPI, UCX, PMIx |
| Performance and topology | hwloc, PAPI, OTF2 |
| Mathematical and numerical tools | Intel oneAPI components and other installed numerical libraries |
| Build systems | CMake, GNU Make, Ninja |
| Containers | Apptainer |

Different versions may be available depending on the compiler and module environment.

For a current software summary, see [Software Availability by System](/software/availability).

## GPU Software Environments

Rostam provides NVIDIA and AMD GPU computing systems.

The primary GPU programming environments are:

| Accelerator | Software Ecosystem |
|---|---|
| NVIDIA V100 | CUDA |
| NVIDIA A100 | CUDA |
| NVIDIA GB10 | CUDA |
| AMD MI100 | ROCm, HIP |

CUDA and ROCm compatibility depends on the device architecture, GPU driver, toolkit version, compiler, and runtime libraries.

A CUDA or ROCm installation detected on one GPU family should not be assumed compatible with every other GPU family.

### NVIDIA CUDA

CUDA modules can be discovered using:

```bash
module spider cuda
```

After loading an appropriate CUDA module, users can inspect the toolkit version:

```bash
nvcc --version
```

The NVIDIA GPU driver can be inspected using:

```bash
nvidia-smi
```

The CUDA version reported by `nvidia-smi` represents the driver-supported CUDA API level, not necessarily the version of the installed CUDA compiler.

### AMD ROCm

ROCm provides GPU runtime and programming support for AMD accelerators.

On Kamand, GPU information can be inspected using:

```bash
rocm-smi
```

Users can discover available ROCm modules with:

```bash
module spider rocm
```

Availability of individual ROCm components depends on the node's software installation.

### GPU Software Compatibility

Users should verify:

- GPU architecture.
- Installed device driver.
- CUDA or ROCm toolkit version.
- Compiler compatibility.
- Required runtime libraries.
- Application target architecture.

For detailed GPU hardware and scheduling instructions, see [Rostam Hardware](/cluster/hardware).

## Containers

Rostam provides Apptainer on the main x86-64 compute environment.

Apptainer enables users to run applications within containerized software environments while integrating with the host operating system and HPC infrastructure.

Check the installed version with:

```bash
apptainer --version
```

Containers can help reproduce application environments, but they do not eliminate CPU architecture or GPU driver compatibility requirements.

For example, an x86-64 container cannot ordinarily execute directly on ARM64 or RISC-V hardware without additional compatibility mechanisms.

GPU-enabled containers must also be compatible with the host's accelerator runtime and device drivers.

## Reproducible Software Environments

Rostam encourages users to record the software environment associated with important computational results.

This includes:

- Operating system and processor architecture.
- Compiler version.
- Loaded environment modules.
- MPI implementation.
- Numerical library versions.
- Python dependencies.
- CUDA or ROCm versions when applicable.
- Slurm job settings.

### Recording Loaded Modules

Within a Slurm job script, the current module environment can be recorded using:

```bash
module list
```

To capture it in a separate file:

```bash
module list 2> modules-used.txt
```

The output can help reproduce the environment or diagnose application failures.

### Recording Compiler Versions

```bash
gcc --version
```

```bash
g++ --version
```

```bash
python3 --version
```

### Recording the Host Environment

```bash
hostname
uname -m
cat /etc/os-release
```

### Recording Slurm Job Information

Within an allocated job:

```bash
scontrol show job "$SLURM_JOB_ID"
```

Keeping this information with research results helps maintain reproducibility across compiler updates, operating-system changes, and different computing architectures.

## Software Availability and Documentation

Rostam's software environment evolves as compilers, libraries, operating systems, and research requirements change.

The hardware architecture and installed software available on a particular system may differ from those on another system.

Before beginning a new project, users should consult:

- [Software Availability by System](/software/availability) — Software inventory by node family.
- [Rostam Hardware Inventory](/cluster/hardware) — Available computing architectures and accelerators.
- [Accessing Rostam](/ssh) — SSH access and getting started.
- [Advanced SSH Configuration](/ssh/advanced_ssh) — SSH keys and compute-node access.

For detailed module information, use `module spider` on the target environment.

### Software Inventory

The software inventory was last surveyed on October 9, 2026.

The inventory describes installed and discoverable software at the time of the survey. Module compatibility and current availability should always be verified on the system where the application will run.
