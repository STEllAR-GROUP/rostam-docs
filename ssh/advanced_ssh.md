---
title: Advaced SSH Configuration
description: 
published: true
date: 2026-10-09T23:51:52.833Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:55.721Z
---

# Advanced SSH Configuration

OpenSSH supports public key authentication, reusable connection aliases, keepalive settings, SSH jump hosts, and port forwarding. This guide assumes you can already connect to [Rostam](/en/ssh). The examples apply to Linux, macOS, and Windows OpenSSH unless otherwise noted.

## Generate an SSH key

Use Ed25519 for new SSH keys:

```bash
ssh-keygen -t ed25519 -C 'rostam'
```

Accept the default location unless it would overwrite an existing key. Your private key is `~/.ssh/id_ed25519` and the shareable public key is `~/.ssh/id_ed25519.pub`. **Protect the private key with a passphrase, never share it, and never put it on a remote server.** An SSH agent can avoid repeated passphrase prompts.

Windows PowerShell uses the same command, with keys ordinarily stored under `%USERPROFILE%\.ssh`. For older compatibility requirements, RSA 4096 (`ssh-keygen -t rsa -b 4096`) may be supported, but Ed25519 is the preferred default.

## Install the public key

If Rostam permits user-managed `authorized_keys`, on Linux (or macOS with `ssh-copy-id` installed):

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub myuser@rostam.cct.lsu.edu
```

On macOS `ssh-copy-id` may be installed via Homebrew (`brew install ssh-copy-id`). Alternatively, from Linux/macOS:

```bash
cat ~/.ssh/id_ed25519.pub | ssh myuser@rostam.cct.lsu.edu   'umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys'
```

On Windows PowerShell, display the public key (`Get-Content "$HOME\.ssh\id_ed25519.pub"`) and install the line through an approved Rostam key-registration method, or use an appropriate SSH command. Set remote SSH permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Where FreeIPA or another centralized SSH key system manages authorization, use the required administrative process instead of assuming `authorized_keys` is accepted.

Test explicitly:

```bash
ssh -i ~/.ssh/id_ed25519 myuser@rostam.cct.lsu.edu
```

SSH keys do **not** necessarily eliminate required [two-factor authentication](/en/ssh/otp).

## SSH agents

On Linux or macOS, an SSH agent can cache your unlocked key:

```bash
ssh-add ~/.ssh/id_ed25519
ssh-add -l
```

If no agent is available in a Linux shell, start one with `eval "$(ssh-agent -s)"`. On macOS, Apple's Keychain integration is available with `ssh-add --apple-use-keychain ~/.ssh/id_ed25519`.

On Windows, an administrator can enable the OpenSSH Authentication Agent service:

```powershell
Set-Service -Name ssh-agent -StartupType Automatic
Start-Service ssh-agent
```

Then, in normal PowerShell:

```powershell
ssh-add "$HOME\.ssh\id_ed25519"
```

## Simplify connections with SSH config

Create the client configuration file at `~/.ssh/config` (Linux/macOS) or `%USERPROFILE%\.ssh\config` (Windows). On Linux/macOS, ensure `~/.ssh` has mode 700 and `config` has mode 600.

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    Port 22
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

Then:

```bash
ssh rostam
scp input.dat rostam:~/
sftp rostam
```

`ServerAliveInterval 60` sends keepalive requests when the connection is idle; `ServerAliveCountMax 3` disconnects after three unanswered requests. This helps with idle network timeouts but cannot make a broken connection survive indefinitely.

Inspect effective settings with `ssh -G rostam`. Use `ssh -v rostam` to troubleshoot.

### Saving Lmod and SSH settings

SSH config is local to your workstation; it is separate from Lmod modules and Slurm. Keep distinct compiler environments in Lmod module collections and SSH host shortcuts in `~/.ssh/config`.

## ProxyJump to a compute node

Most compute nodes are on internal networks, and access may be restricted by `pam_slurm_adopt`. ProxyJump routes SSH through the login host, but **does not grant a Slurm allocation**.

Example configuration for the NVIDIA V100 node `bahram`:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519

Host bahram
    HostName bahram.rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519
    ProxyJump rostam
```

First connect to the login host and request an allocation on Bahram:

```bash
ssh rostam
salloc -p cuda-V100 -N 1 --nodelist=bahram --gres=gpu:1 -t 01:00:00
```

After Slurm grants the allocation, leave it active and open a **second local terminal**:

```bash
ssh bahram
hostname
```

If no allocation exists, the node may respond:

```text
Access denied by pam_slurm_adopt: you have no active jobs on this node
```

After finishing, `exit` the compute-node shell and `exit` the allocating `salloc` session to release resources. Direct SSH to a node is subject to site policy; for ordinary interactive computing, prefer Slurm itself:

```bash
srun -p cuda-V100 -N 1 --gres=gpu:1 -t 01:00:00 --pty /bin/bash -l
```

On Windows, the same OpenSSH config works in PowerShell. PuTTY and MobaXterm can use a gateway/jump-host configuration, but exact menus depend on client versions. GUI clients still require a valid allocation.

## Port forwarding

For an authorized service running on an allocated node (for example on TCP port 8888), forward a local port:

```bash
ssh -N -J rostam -L 8888:localhost:8888 myuser@bahram.rostam.cct.lsu.edu
```

Then connect locally to `http://localhost:8888`. This requires the service to be running on Bahram and SSH tunneling to be permitted. Never use forwarding to bypass authentication or expose restricted services.

## Diagnose problems

```bash
ssh -v rostam
ssh -G bahram
ssh-add -l
squeue -u "$USER"
```

If you see a changed SSH host-key warning, verify the fingerprint with administrators before accepting it. If many agent keys cause `Too many authentication failures`, `IdentitiesOnly yes` can limit what is offered.

For a persistent terminal environment after transient disconnects, use `tmux` (where installed); long computations belong in Slurm batch jobs.

## Related pages

- [Connecting to Rostam](/en/ssh)
- [File Transfers](/en/ssh/file-transfers)
- [Two-Factor Authentication](/en/ssh/otp)
- [Hardware Resources](/en/cluster/hardware)
- [Running GPU Jobs](/en/slurm/gpu)
