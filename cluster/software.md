---
title: Software Library
description: 
published: true
date: 2020-06-23T04:34:35.092Z
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
|gcc      |8.3       |
|glibc    |2.28      |
|binutils |2.30      |

## Software Library

