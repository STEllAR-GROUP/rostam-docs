---
title: Connecting to Rostam
description: How to connect to Rostam via ssh
published: true
date: 2026-10-09T23:51:36.338Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:37.371Z
---

# Connecting to Rostam

Rostam is accessed remotely through **Secure Shell (SSH)**. SSH provides encrypted terminal access and also supports file transfers using SFTP and SCP. The public login hostname is:

```text
rostam.cct.lsu.edu
```

Use your assigned Rostam username in place of `myuser`:

```bash
ssh myuser@rostam.cct.lsu.edu
```

SSH uses port 22 by default. Your Rostam username may differ from your LSU username. Two-factor authentication (2FA) or additional prompts may be required; see [Two-Factor Authentication Setup](/en/ssh/otp).

## Linux

Most Linux distributions include the OpenSSH client. Open Terminal and run:

```bash
ssh myuser@rostam.cct.lsu.edu
```

If needed, install `openssh-clients` on Fedora/RHEL/AlmaLinux (`sudo dnf install openssh-clients`) or `openssh-client` on Debian/Ubuntu (`sudo apt install openssh-client`).

## macOS

Open **Terminal** (Applications → Utilities → Terminal) or another terminal such as iTerm2, then run:

```bash
ssh myuser@rostam.cct.lsu.edu
```

The built-in SSH client is sufficient. No commercial client is needed.

## Windows

**PowerShell or Windows Terminal:** modern Windows includes or can enable the OpenSSH Client. Check with `ssh -V`, then connect:

```powershell
ssh myuser@rostam.cct.lsu.edu
```

**[PuTTY](https://www.putty.org/):** enter host `rostam.cct.lsu.edu`, port `22`, and connection type SSH; open the session and enter your username.

**[MobaXterm](https://mobaxterm.mobatek.net/):** Session → SSH → remote host `rostam.cct.lsu.edu` → specify username. MobaXterm also provides an SFTP pane and a built-in X server.

Other supported options include [Bitvise](https://www.bitvise.com/ssh-client), [SecureCRT](https://www.vandyke.com/products/securecrt/), [Termius](https://termius.com/), and OpenSSH under Windows Subsystem for Linux. Visual Studio Code Remote - SSH can be useful for editing remote source code when compatible with site policies.

## First login and host verification

On a first connection, SSH may ask you to accept the host's public key. **Verify the fingerprint with a trusted Rostam administrator/source before accepting it.** SSH remembers accepted keys to detect unexpected server identity changes. Do not disable strict host-key checking to work around a warning.

Complete the required password, key, and/or 2FA prompts. To disconnect, run `exit` or press Ctrl+D.

## Using the login environment

Use the login node for editing scripts, managing files, reviewing results, loading modules, and submitting jobs. Run CPU- or GPU-intensive computations on allocated compute nodes through Slurm.

```bash
whoami
pwd
module avail
sinfo
squeue -u "$USER"
```

To request a one-hour interactive compute shell:

```bash
srun -N 1 -t 01:00:00 --pty /bin/bash -l
```

## File transfers

SSH also supports encrypted transfers. From your **local** computer:

```bash
scp input.dat myuser@rostam.cct.lsu.edu:~/
scp myuser@rostam.cct.lsu.edu:~/results.dat .
sftp myuser@rostam.cct.lsu.edu
```

Graphical alternatives include WinSCP, MobaXterm, FileZilla, and Cyberduck. For step-by-step instructions and rsync, see [File Transfers](/en/ssh/file-transfers).

## Graphical applications (X11)

On Linux with a suitable local X server:

```bash
ssh -X myuser@rostam.cct.lsu.edu
```

On macOS, install and run [XQuartz](https://www.xquartz.org/) before using X11 forwarding. On Windows, MobaXterm supplies an X server; PuTTY can be configured for forwarding with a local X server such as VcXsrv. Trusted forwarding (`ssh -Y`) is less restrictive and should only be used when necessary and with trusted servers. Check `echo "$DISPLAY"` after login. Use Slurm allocations for resource-intensive graphical software.

## Troubleshooting

- **Timeout:** verify hostname, port 22, network/VPN requirements, and reachability.
- **Permission denied:** confirm username, active account, SSH key, and 2FA setup.
- **Host identification changed:** verify the new key with administrators; do not blindly delete the old fingerprint.
- **Unstable connections:** use a terminal multiplexer such as `tmux` for persistent terminal work; submit computations through Slurm.

Verbose SSH diagnostic output:

```bash
ssh -v myuser@rostam.cct.lsu.edu
```

## Next steps

- [Two-Factor Authentication](/en/ssh/otp)
- [Advanced SSH Configuration](/en/ssh/advanced_ssh) — keys, agents, aliases, and ProxyJump
- [File Transfers](/en/ssh/file-transfers) — SCP, SFTP, rsync, and GUIs
- [Hardware Resources](/en/cluster/hardware)
- [Software Environment](/en/cluster/software)
