---
title: Singularity
description: 
published: true
date: 2020-09-08T05:09:06.691Z
tags: 
editor: markdown
---

## Introduction

Containerization of workloads has become popular, particularly using [Docker](https://www.docker.com/). However, Docker is not suitable for HPC applications due to security reasons. There are a couple of alternatives for HPC containers, with [Singularity](https://sylabs.io/singularity/) being the one that covers a large set of cases. Singularity has been deployed on the cluster, and can also import Docker containers.

Containers were created to isolate applications from the host environment. This means that all necessary dependencies are packaged into the application itself, allowing the application to run anywhere containers are supported. With container technology, administrators are no longer bogged down supporting every tool and library under the sun, and developers have complete control over the environment their tools ship with.
Container technologies

Even if you haven’t run or built a docker container, you have probably heard of the technology. Docker has become extremely popular for both applications and services, but it requires elevated privileges, making it a security risk for shared servers. Singularity was designed to run without root privileges while also providing access to host devices, making it a good fit for traditional HPC environments.

## Why Singularity? (and Why not Docker?)

There are some important differences between Docker and Singularity:

* Docker and Singularity have their own container formats.
* Docker containers may be imported to run via Singularity.
* Docker containers need root privileges for full functionality which is not suitable for a shared HPC environment.
* Singularity allows working with containers as a regular user.

|																									|Docker |Singularity |
|-------------------------------------------------|:-----:|:----------:|
|Runs docker containers     											| Yes 	| Yes				 |
|Edits docker containers     											| Yes 	| Yes				 |
|Interacts with host devices											| Yes 	| Yes				 |
|Interacts with host filesystems  								| Yes 	| Yes				 |
|Runs without sudo																| No  	| Yes				 |
|Runs as host user																| No  	| Yes				 |
|Can become root in container											| Yes 	| No			 	 |
|Control network interfaces												| Yes 	| No				 |
|Configurable capabilities for enhanced security	| No 		| Yes 			 |

## Using Singularity

The most up-to-date help on Singularity comes from the command itself.

````bash
[rostam0 ~]$ singularity --help

Linux container platform optimized for High Performance Computing (HPC) and
Enterprise Performance Computing (EPC)

Usage:
  singularity [global options...]

Description:
  Singularity containers provide an application virtualization layer enabling
  mobility of compute via both application and environment portability. With
  Singularity one is capable of building a root file system that runs on any 
  other Linux system where Singularity is installed.

Options:
  -c, --config string   specify a configuration file (for root or
                        unprivileged installation only) (default
                        "/etc/singularity/singularity.conf")
  -d, --debug           print debugging information (highest verbosity)
  -h, --help            help for singularity
      --nocolor         print without color output (default False)
  -q, --quiet           suppress normal output
  -s, --silent          only print errors
  -v, --verbose         print additional information
      --version         version for singularity

Available Commands:
  build       Build a Singularity image
  cache       Manage the local cache
  capability  Manage Linux capabilities for users and groups
  config      Manage various singularity configuration (root user only)
  delete      Deletes requested image from the library
  exec        Run a command within a container
  help        Help about any command
  inspect     Show metadata for an image
  instance    Manage containers running as services
  key         Manage OpenPGP keys
  oci         Manage OCI containers
  plugin      Manage Singularity plugins
  pull        Pull an image from a URI
  push        Upload image to the provided URI
  remote      Manage singularity remote endpoints
  run         Run the user-defined default command within a container
  run-help    Show the user-defined help for an image
  search      Search a Container Library for images
  shell       Run a shell within a container
  sif         siftool is a program for Singularity Image Format (SIF) file manipulation
  sign        Attach digital signature(s) to an image
  test        Run the user-defined tests within a container
  verify      Verify cryptographic signatures attached to an image
  version     Show the version for Singularity

Examples:
  $ singularity help <command> [<subcommand>]
  $ singularity help build
  $ singularity help instance start


For additional help or support, please visit https://www.sylabs.io/docs/
````

## Getting Existing Images

You can also use the pull or build commands to download pre-built images from external resources, such as [Singularity Hub](https://singularity-hub.org/), the [Sylabs Container Library](https://cloud.sylabs.io/library) or [Docker Hub](https://hub.docker.com/).

````bash
[rostam0 ~]$ singularity pull shub://vsoch/hello-world
INFO:    Downloading shub image
59.8MiB / 59.8MiB [==============================================] 100 % 45.9 MiB/s 0s
````

You can give your image a different while downlaoding the image:

````bash
[rostam0 ~]$ singularity pull --name hello.sif shub://vsoch/hello-world
INFO:    Downloading shub image
59.8MiB / 59.8MiB [==============================================] 100 % 45.9 MiB/s 0s
````

Similarly, you can pull images from Docker Hub:

````bash
[rostam0 ~]$ singularity pull docker://godlovedc/lolcow
INFO:    Converting OCI blobs to SIF format
INFO:    Starting build...
...
Writing manifest to image destination
Storing signatures
INFO:    Creating SIF file...
````

## Working with Images

When working with the images you could either work on the headnode or start an interactive session

### Shell

With the `shell` command, you can start a new shell within the container image and interact with it as if it were a small virtual machine.

````bash
[rostam0 ~]$ singularity shell hello-world_latest.sif 
Singularity> ls
...
Singularity> cat /etc/os-release 
NAME="Ubuntu"
VERSION="14.04.6 LTS, Trusty Tahr"
...
Singularity> exit
````

### Commands within a Container

You can use the `exec` command to execute specific commands within the container:

````bash
[rostam0 ~]$ singularity exec hello-world_latest.sif cat /etc/os-release
NAME="Ubuntu"
VERSION="14.04.6 LTS, Trusty Tahr"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 14.04.6 LTS"
VERSION_ID="14.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
````

### Running Containers

Singularity images contain run-scripts that can be triggered with the `run` command to perform specific actions when the container is run. This can be done either by using the run command, or by calling the container as if it were an executable,

````bash
[rostam0 ~]$ singularity run hello-world_latest.sif 
RaawwWWWWWRRRR!! Avocado!
````

or

````bash
[rostam0 ~]$ ./hello-world_latest.sif 
RaawwWWWWWRRRR!! Avocado!
````

### Accessing Files from a Container

Files and directories on the cluster are accessible from within the container. By default, `$HOME` (your home directory), `$PWD` (your current directory), and `/tmp` are available at runtime.

You can specify additional directories to bind mount into your container with the `--bind` option. For instance, in the below example your `work` directory is bind mounted to the `/work` directory inside the container:

````bash
[rostam0 ~]$ singularity exec --bind $WORK:/work hello-world_latest.sif cat /work/hello.txt 
Hello from the Container!
````

## Singularity Containers as SLURM Jobs
You can also use Singularity images within an interactive, non-interactive or a batch job. All the commands described above could be executed as Slurm jobs.

### Non-interactive Job

````bash
[rostam0 ~]$ srun -p medusa -N 1 singularity run hello-world_latest.sif
RaawwWWWWWRRRR!! Avocado!
````

````bash
[rostam0 ~]$ srun -p medusa -N 1 singularity exec hello-world_latest.sif cat /etc/os-release
NAME="Ubuntu"
VERSION="14.04.6 LTS, Trusty Tahr"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 14.04.6 LTS"
VERSION_ID="14.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
````

````bash
[rostam0 ~]$ srun -p medusa -N 1 singularity exec --bind $WORK:/work hello-world_latest.sif cat /work/hello.txt
Hello from the Container!
````

### Interactive Job

Running Singularity shell:

````bash
[rostam0 ~]$ srun -p medusa -N 1 --pty singularity shell hello-world_latest.sif
Singularity> hostname 
medusa00.rostam.cct.lsu.edu
Singularity> cat /etc/os-release 
NAME="Ubuntu"
VERSION="14.04.6 LTS, Trusty Tahr"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 14.04.6 LTS"
VERSION_ID="14.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
Singularity> exit
exit
````

### Batch Job

The same way you can create a batch job for your non-interactive jobs:

````bash
#!/bin/bash

#SBATCH --job-name=singularity_test
#SBATCH --output=singularity_test.out
#SBATCH --partition=medusa
#SBATCH --nodes=2
#SBATCH --time=10:00

srun singularity exec hello-world_latest.sif cat /etc/os-release
````
