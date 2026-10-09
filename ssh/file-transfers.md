---
title: File Transfers
description: 
published: true
date: 2026-10-09T23:52:46.288Z
tags: 
editor: markdown
dateCreated: 2026-10-09T23:52:46.288Z
---

# File Transfers

Use SSH-based file transfers to move source code, datasets, scripts, and results between your computer and Rostam. Prefer **SFTP, SCP, or rsync over SSH** rather than unencrypted FTP.

| Setting | Value |
|---|---|
| Host | `rostam.cct.lsu.edu` |
| Port | `22` |
| Protocol | SFTP (preferred for graphical clients) or SCP |
| Username | Your assigned Rostam username |
| Authentication | Approved SSH credentials and any required 2FA |

For first-time SSH setup, see [Connecting to Rostam](/en/ssh). SSH keys and aliases are documented at [Advanced SSH Configuration](/en/ssh/advanced_ssh).

## Choose a tool

| Tool | Platforms | Best use |
|---|---|---|
| `scp` | Linux, macOS, Windows OpenSSH | Simple file/directory copies |
| `sftp` | Linux, macOS, Windows OpenSSH | Interactive transfers |
| `rsync` over SSH | Linux, macOS, WSL / installed rsync | Incremental synchronization |
| [WinSCP](https://winscp.net/) | Windows | Graphical two-pane transfer manager |
| [MobaXterm](https://mobaxterm.mobatek.net/) | Windows | SSH terminal plus SFTP browser |
| [FileZilla](https://filezilla-project.org/) | Linux, macOS, Windows | Cross-platform SFTP GUI |
| [Cyberduck](https://cyberduck.io/) | macOS, Windows | Graphical SFTP and bookmarks |
| [Bitvise](https://www.bitvise.com/ssh-client) | Windows | SSH plus graphical file transfer |

## SCP: copy files from a local terminal

Run the following commands on **your workstation**, not after logging into Rostam.

Upload a file:

```bash
scp input.dat myuser@rostam.cct.lsu.edu:~/
```

Download a result into the current local directory (`.`):

```bash
scp myuser@rostam.cct.lsu.edu:~/results.dat .
```

Upload an entire directory:

```bash
scp -r myproject/ myuser@rostam.cct.lsu.edu:~/
```

On Windows PowerShell, ordinary OpenSSH `scp` commands work, and local paths may be quoted:

```powershell
scp "$HOME\Downloads\input.dat" myuser@rostam.cct.lsu.edu:~/
```

## SFTP: interactive file management

Start:

```bash
sftp myuser@rostam.cct.lsu.edu
```

| SFTP command | Action |
|---|---|
| `pwd`, `ls` | Remote directory and listing |
| `lpwd`, `lls` | Local directory and listing |
| `cd`, `lcd` | Change remote or local directory |
| `put file` | Upload |
| `get file` | Download |
| `put -r dir`, `get -r dir` | Recursively transfer a directory |
| `bye` | Exit |

Example:

```text
sftp> cd project
sftp> put input.dat
sftp> get output.dat
sftp> bye
```

## rsync: large or repeated transfers

For an application directory that changes frequently:

```bash
rsync -avh --progress --partial myproject/   myuser@rostam.cct.lsu.edu:~/myproject/
```

Download only changed results:

```bash
rsync -avh --progress myuser@rostam.cct.lsu.edu:~/results/ ./results/
```

Preview an operation without changes:

```bash
rsync -avhn myproject/ myuser@rostam.cct.lsu.edu:~/myproject/
```

A trailing slash on the source means **copy its contents**, not the enclosing directory. Be careful with `--delete`: it removes destination files not present in the source. `rsync` must be installed at both ends. Windows users can use it in WSL.

## Windows: WinSCP

1. Install [WinSCP](https://winscp.net/).
2. Create a new site: **SFTP**, host `rostam.cct.lsu.edu`, port **22**, assigned username.
3. Verify the server's SSH host-key fingerprint and complete authentication.
4. Drag files between local and remote panes, or use Upload/Download and synchronization tools.

Some authentication methods may require recent client versions or keyboard-interactive authentication support. Do not disable verification to work around a prompt.

## Windows: MobaXterm

1. In [MobaXterm](https://mobaxterm.mobatek.net/), select Session → SSH.
2. Set remote host and username, and connect.
3. After authentication, use the SFTP browser associated with the SSH session to upload, download, or edit files.

MobaXterm also supplies a terminal and X11 server, useful for HPC work.

## Linux, macOS, or Windows: FileZilla

1. Install [FileZilla Client](https://filezilla-project.org/).
2. In Site Manager, create a site using **SFTP - SSH File Transfer Protocol**.
3. Set host `rostam.cct.lsu.edu`, port 22, and your account's approved login method.
4. Verify the host key and authenticate; drag files between local and remote panes.

## macOS or Windows: Cyberduck

1. Install [Cyberduck](https://cyberduck.io/).
2. Open Connection → **SFTP**.
3. Enter `rostam.cct.lsu.edu`, port 22, and username.
4. Authenticate and use upload/download actions or saved bookmarks.

Other SSH-capable transfer clients may be used provided they support the required authentication and host-key verification.

## Large datasets

For repeated transfers, prefer `rsync` over copying everything from scratch. For many small files, consider archiving them first, if storage capacity and workload policies permit:

```bash
tar -czf results.tar.gz results/
```

Large or heavy filesystem operations should use the cluster's approved storage and data-transfer policies rather than overloading a login host. Avoid placing large datasets in home directories without checking storage policies and quotas.

## Troubleshooting

- **Permission denied:** confirm source readability, destination write permissions, username, and 2FA.
- **No such file:** check the local/remote side of the colon in `scp` and quote paths containing spaces.
- **Timeout:** test basic SSH to port 22; check network or VPN requirements.
- **Graphical client fails at 2FA:** verify it supports keyboard-interactive authentication; test terminal SSH first.
- **Host-key changed:** verify the fingerprint with administrators before accepting a change.
- **Interrupted copy:** retry with rsync or a GUI that supports resuming.

Check remote filesystem capacity with `df -h`; individual quotas may require a separate command.

Related: [Connecting to Rostam](/en/ssh) · [Advanced SSH](/en/ssh/advanced_ssh) · [Software Environment](/en/cluster/software).
