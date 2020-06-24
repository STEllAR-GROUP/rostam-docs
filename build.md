---
title: Compiling Your Program
description: 
published: true
date: 2020-06-24T03:42:37.369Z
tags: 
editor: markdown
---

Building a software is the process of producing a machine executable code from source files. The simplest form of it, is a one-line call to the compiler. But  more commonly, on Linux systems a [makefile](http://www.gnu.org/software/make/manual/make.html) is used to build the code systematically. However more increasingly the larger projects choose to use a multi-step automated build system managed by tools like [autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html) or [cmake](https://cmake.org/). 

In STE||AR group we use `cmake` for our build system. Since many general guides are available on how to biuld your program (our favorite one is [this one](https://frontera-portal.tacc.utexas.edu/user-guide/building/) form TACC portal), we decided to avoid repeating the instructions and only mention the important ones.

## Building HPX on Rostam

The first setup is to setup your development environment by loading appropriate [modules](/cluster/software#environment-module). Rostam provides three sets of compilers; [gcc](https://gcc.gnu.org/), [llvm/clang](http://llvm.org/) and [Intel](https://software.intel.com/content/www/us/en/develop/tools/compilers.html). Choose your weapon wisely. For this guide you we use `llvm` compiler, for the others the instructions are similar.

Loading the compiler:

```bash
module load clang		# Load default version of clang
# This set all variable and flags necessary to build HPX
```

Then load other HPX prerequisites, the list could vary based on your components and flags. These are the usual ones:

```bash
module load boost cmake git papi hwloc
```

> It is a good practice that from early on, you organize your *home* and *work* directtories. You are allocated 50GB in `$HOME` and 1TB in `$WORK` directories. There is no performance difference between the two. Keep the important codes and data in *home* and move large libraries and data to *work*.{.is-success}

> Use `cdw` command to quickly switch to your work directory. {.is-success}

Let's assume you want to use *work* directory and you want to keep the source files in `source` directory and compiled programs in a directory named `apps`. First we create those directories and clone HPX in `source`:

```bash
mkdir $WORK/{source,apps}
cd $WORK/source
git clone https://github.com/STEllAR-GROUP/hpx.git
```

Now you have a fresh copy of HPX in `$WORK/source/hpx`. HPX's cmake requires to run cmake command on a separate directory than source code, it doesn't matter this *build* directory is inside or outdide the source directory just it can't be the same as the source. Let's make a build directory inside source directory and do our business there.

```bash
mkdir $WORK/source/hpx/build
cd $WORK/source/hpx/build
```

Now we are ready for our first step of the build porcess, which is the `cmake` command. The cmake will read the instructions and generate the next step of the build process. By default on linux systems, cmake will generate makefile. Let's go with that:

```bash
cmake \
-DCMAKE_BUILD_TYPE=Release \
-DHPX_WITH_THREAD_IDLE_RATES=ON \
-DCMAKE_INSTALL_PREFIX=$WORK/apps/hpx-$(date +'%Y%m%d') \
$WORK/source/hpx/
```

Let's go over the command options one by one:

`-DCMAKE_BUILD_TYPE=Release` sets the build type flag. The options are `Debug`, `Release`, `RelWithDebInfo`.

The `DHPX_WITH_THREAD_IDLE_RATES=ON` is an ontional flag but a good one to have. It is an example of HPX options, built-in on its cmake configuration. We placed it here to show the process.

The `-DCMAKE_INSTALL_PREFIX=` flag sets the installation destination. Here we chose `$WORK/apps/hpx-` `+` `today's date`.

>It's a good practice to note the date you cloned the master to keep a record of how old your library is. If found it annoyting to keep the track of your libraries path, you can create a soft link to hide the dates when using the library; `ln -s $WORK/apps/hpx-$(date +'%Y%m%d') $WORK/apps/hpx`
{.is-success}

If everyting goes according to the plan, cmake should populate your build directory (your current directory) with necessary makefiles to build HPX. Next step would be to invoke the `make` command to compile HPX, here we instruct make to use 12 concurrent jobs as an example:

```bash
make -j 12
```

In last step we invoke `make install` to copy the binary files to its destination:

```bash
make install
```

Voila! You successfully compiled HPX. Let's give it a test run:

```bash
$ cd $WORK/apps/hpx-$(date +'%Y%m%d')
$ ./bin/hello_world_1
Hello World!
```

## Building Phylanx on Rostam