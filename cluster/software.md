---
title: Software Library
description: 
published: true
date: 2020-06-23T17:49:57.253Z
tags: 
editor: markdown
---

Rostam is built with the mindset that it sould create a friendly environment for students who want to expand their knowledge, while at the same time it could be used by exprienced programmers as a test bed, before they move to a bigger cluster where the real computation happens.

While Rostam tries to keep itself up do date with the latest developments, it also tries to keep the conventional tools and configuration in place. The small size of the cluster and number of its users make it possible to better accomodate users' needs.

## CentOS 8
Many academic cluster today use *Red Hat Enterprise Linux (RHEL)* or its cloned brother, the *Community Enterprise Operating System* **(CentOS)**. While many clusters stayed with version 7 of the OS, early on we decided to move to CentOS 8 and continue to upgrade the minor version (currently version 8.2)

The main problem on *RHEL/CentOS version 7* was the dual ABI problem in *gcc*. The gcc prior version 5.1 used the old ABI or better to say the newer version use the new ABI which is not compatible with each other. *RHEL/CentOS 7* is compiled with *gcc 4.8* and comes with that compiler, therefore all libraries are compiled with the old ABI. As it is obvius, the *gcc 4.8* is ancient and lacks certain features, to have a new version of the compiler we had to compile our own compiler. It is possible configure the newer gcc to use old ABI, but this was agaist the whole point of having a new compiler. On the other hand the new ABI means the binaries become incompatible, so we had to compile many other libraries with new ABI to make our own platform. Many systems may opt to use the old ABI for all compiler, we found it easier to jump forward and use the new ABI for every library.

The base system on the cluster is:

|Name     |Version   |
|---------|:--------:|
|CentOS   |8.2.2004  |
|Kernel   |4.18      |
|gcc      |8.3       |
|glibc    |2.28      |
|binutils |2.30      |

## Software Library

Many software libraris and application are provided by official CentOS repositories or supporded thrid-parties. Also we have compiled many more libraries to satisfy the needs of developers and researchers. In general softwares available on Rostam could be divided in three categories:

- Software provided by official repositories. These softwares are available throughout the system and usually require no special configuration to use. (i.e. fish, singularity, vim ...)
- Softwares compiled by admins using default compiler on the system. To used these libraries you should load the appropriate [Environment Module](#Environment-Module). (i.e. cmake, gcc, clang ...)
- Softwares that are compiled with specific compilers or flags, to use those software aside from loading the modules you must also load the dependencies. To avoid any confusion, the modules for these applications are hidden and only appear when the prerequisite module is loaded. You can use module spider to locate such applications (i.e. boost-clang, boost-gcc ...)

## Environment Module
Rostam uses `Lmod`, a module system developed and maintained at TACC, to manage your environment so you have access to the software packages and versions that you need to conduct your research. Loading a module amounts to choosing a specific package from among available alternatives:

```bash
module load clang          # load the default Clang compiler v10.0.0
module load clang/9.0.1    # load a specific version of the Clang compiler
```

A module does its job by defining or modifying environment variables (and sometimes aliases and functions). For example, a module may prepend appropriate paths to `$PATH` and `$LD_LIBRARY_PATH` so that the system can find the executables and libraries associated with a given software package. The module creates the illusion that the system is installing software for your personal use. Unloading a module reverses these changes and creates the illusion that the system just uninstalled the software:

```bash
module load hwloc      # defines hwloc-related env vars; modifies others
module unload hwloc    # undoes changes made by load
```

The module system does more, however. When you load a given module, the module system can automatically replace or deactivate confilicing modules to ensure the packages you have loaded are compatible with each other. In the example below, the module system automatically unloads one compiler when you load another, It also reloads the dependent library when available:

```bash
$ module load gcc/9.3.0
$ module load boost/1.72.0-release
$ module load clang/9.0.1 

Lmod is automatically replacing "gcc/9.3.0" with "clang/9.0.1".

Due to MODULEPATH changes, the following have been reloaded:
  1) boost/1.72.0-release

```

To see the modules you currently have loaded:

```bash
module list
```

To see all modules that you can load right now because they are compatible with the currently loaded modules:

```bash
module avail
```

To see all installed modules, even if they are not currently available because they are incompatible with your currently loaded modules:

```bash
module spider   # list all modules, even those not available to load
```

To filter your search:

```bash
module spider boost             				# all modules with names containing 'boost'
module spider sundials/1.72.0-release   # additional details on a specific module
```

You can save a collection of modules as a personal default collection that will load every time you log into Frontera. To do so, load the modules you want in your collection, then execute:

```bash
module save    # save the currently loaded collection of modules 
```

Two commands make it easy to return to a known, reproducible state:

```bash
module reset   		# load the system default collection of modules
module restore 		# load your personal default collection of modules
```

On Rostam, the command `module reset` is equivalent to `module purge; module load Rostam2`. It's a safer, easier way to get to a known baseline state than issuing the two commands separately.

Help text is available for both individual modules and the module system itself:

```bash
module help blaze     # show help text for software package blaze
module help         	# show help text for the module system itself
```

## Python Library
CentOS 8 provides Python 3.6 which is installed on the cluster. Also we always provide a newer version of Python in our module system:

```bash
$ module load python
$ python3 --version
Python 3.8.2
```

This provides you a minimal Python library. You can install any additional package with `pip`.

``bash
pip install numpy
```
> `pip` on Rostam is configured to always use `--user` when invoked by users. All packages will installed in your home directory. You can install, remove, upgrade and downgrade packages when you choose with no worries.{.is-info}

> Rostam does not have any Python 2 library.{.is-warning}
