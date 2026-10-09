---
title: Connecting to Rostam
description: How to connect to Rostam via ssh
published: true
date: 2026-10-09T19:20:33.799Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:37.371Z
---


# Accessing Rostam

Rostam is a high-performance computing (HPC) cluster at the LSU Center for Computation & Technology (CCT). Users connect to Rostam remotely using **Secure Shell (SSH)**, which provides encrypted access to the cluster from Linux, macOS, and Windows computers.

SSH allows you to log in, execute commands, manage files, develop software, and submit computational jobs using Slurm. SSH also supports secure file transfers through tools such as `scp` and `sftp`.

## Before You Connect

You will need:

- An active Rostam user account.
- Your assigned username and authentication credentials.
- An SSH client installed on your computer.
- Two-factor authentication (2FA) configured as required for your account.
- A network connection that can reach Rostam.

The SSH hostname for Rostam is:

```text
rostam.cct.lsu.edu
```

The default SSH port is `22`.

Throughout this guide, replace `myuser` with your own Rostam username.

For example, if your username is `jsmith`, your SSH login command would be:

```bash
ssh jsmith@rostam.cct.lsu.edu
```

> **Note:** Your Rostam username may differ from your LSU username. Use the credentials assigned to your Rostam account.

## Connecting from Linux

Most Linux distributions include the OpenSSH client by default.

Open a terminal and enter:

```bash
ssh myuser@rostam.cct.lsu.edu
```

If OpenSSH is not installed, you can install it using your distribution's package manager.

For Fedora, Red Hat, Rocky Linux, or similar distributions:

```bash
sudo dnf install openssh-clients
```

For Ubuntu or Debian:

```bash
sudo apt install openssh-client
```

### First Connection

The first time you connect, SSH may display a message similar to:

```text
The authenticity of host 'rostam.cct.lsu.edu' can't be established.
ED25519 key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

This is normal when connecting to a server for the first time.

SSH uses host keys to verify the identity of the remote server. Before accepting a new host key, verify its fingerprint against a trusted value provided by the Rostam administrators.

Once verified, enter:

```text
yes
```

Your SSH client will remember the server's host key for future connections.

You will then be prompted to complete the applicable authentication steps.

### Ending Your Session

To disconnect from Rostam:

```bash
exit
```

You can also press `Ctrl+D` at the shell prompt.

## Connecting from macOS

macOS includes an OpenSSH client, so no additional software is normally required.

Open the **Terminal** application, located under:

```text
Applications → Utilities → Terminal
```

Alternatively, use Spotlight Search to find Terminal.

Connect using:

```bash
ssh myuser@rostam.cct.lsu.edu
```

Complete the authentication prompts to establish your session.

The same SSH commands used on Linux generally work on macOS.

### Other macOS Terminal Applications

Users who prefer an alternative terminal can use:

- [iTerm2](https://iterm2.com/) — A feature-rich terminal application for macOS.
- [Warp](https://www.warp.dev/) — A modern terminal application with additional command-line features.
- [Termius](https://termius.com/) — A cross-platform SSH client with graphical connection management.

These applications are optional. Apple's built-in Terminal is sufficient for normal Rostam access.

## Connecting from Windows

Modern Windows systems provide several ways to connect to Rostam.

The simplest approach is to use the built-in SSH client through **PowerShell** or **Windows Terminal**.

Other popular options include PuTTY, MobaXterm, Bitvise, and SecureCRT.

### Option 1: PowerShell or Windows Terminal

<https://learn.microsoft.com/en-us/windows/terminal/>

Windows 10 and Windows 11 support OpenSSH, although the optional feature may need to be enabled on some systems.

Open PowerShell or Windows Terminal and enter:

```powershell
ssh myuser@rostam.cct.lsu.edu
```

Complete the authentication prompts.

This provides essentially the same command-line experience as SSH on Linux or macOS.

To check whether OpenSSH is available:

```powershell
ssh -V
```

If SSH is not recognized, install the **OpenSSH Client** optional Windows feature through Windows Settings.

PowerShell is an excellent choice for users who only need terminal access and basic file transfers.

### Option 2: PuTTY

<https://www.putty.org/>

PuTTY is a widely used, free SSH client for Windows.

It provides a graphical interface for configuring SSH connections.

To connect:

1. Download and install PuTTY from its official website.
2. Launch PuTTY.
3. Enter `rostam.cct.lsu.edu` in the **Host Name** field.
4. Set the port to `22`.
5. Select **SSH** as the connection type.
6. Click **Open**.
7. Verify the server's host key fingerprint when prompted.
8. Enter your Rostam username and complete authentication.

PuTTY allows connection settings to be saved for future use.

Enter a name such as `Rostam` under **Saved Sessions** and click **Save**.

For subsequent connections, select the saved session and click **Open**.

PuTTY also includes additional utilities:

- **PuTTYgen** — SSH key generation and management.
- **PSCP** — Secure file copying.
- **PSFTP** — Interactive SFTP file transfers.
- **Pageant** — SSH authentication agent for managing private keys.

PuTTY is a good option for users who prefer a dedicated graphical SSH client.

### Option 3: MobaXterm

<https://mobaxterm.mobatek.net/>

MobaXterm is a Windows terminal application designed for remote computing environments.

It combines several useful tools:

- SSH terminal sessions.
- Integrated SFTP file browsing.
- Built-in X11 server for graphical Linux applications.
- Multiple terminal tabs.
- SSH session management.
- File transfer and remote system utilities.

MobaXterm is particularly convenient for HPC users because it integrates terminal access, graphical application support, and remote file management.

To connect:

1. Launch MobaXterm.
2. Click **Session**.
3. Select **SSH**.
4. Enter `rostam.cct.lsu.edu` as the remote host.
5. Enable **Specify username** and enter your Rostam username.
6. Leave the port at `22`.
7. Click **OK**.
8. Verify the host key and complete authentication.

MobaXterm can save your connection settings for future sessions.

#### Integrated SFTP Browser

When connected over SSH, MobaXterm can display a graphical SFTP file browser alongside the terminal.

This allows you to transfer files between your Windows computer and Rostam without opening another application.

You can typically upload or download files by dragging them between the file browser and your local computer.

#### Graphical Applications

MobaXterm includes an X11 server and supports SSH X11 forwarding.

This makes it useful for users who need to run graphical Linux applications remotely.

Graphical applications should only be launched on compute nodes allocated through Slurm when they consume significant resources.

### Option 4: Other Windows SSH Clients

Several additional SSH clients are available.

| Application | Description |
|---|---|
| [Bitvise SSH Client](https://www.bitvise.com/ssh-client) | Windows SSH client with terminal and graphical SFTP support |
| [SecureCRT](https://www.vandyke.com/products/securecrt/) | Commercial SSH client with extensive session management |
| [Termius](https://termius.com/) | Cross-platform SSH client with connection management |
| [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/) | Provides a Linux environment within Windows, including standard OpenSSH utilities |
| [Visual Studio Code](https://code.visualstudio.com/docs/remote/ssh) | Supports remote development through the Remote - SSH extension |

For most Windows users, PowerShell is sufficient for basic terminal access, while MobaXterm is especially convenient when graphical applications or integrated file transfers are needed.

## Two-Factor Authentication (2FA)

Access to LSU computing systems requires two-factor authentication where enforced by university and cluster security policies.

Two-factor authentication adds an additional verification step beyond a password.

Depending on your account configuration, SSH authentication may require a password, an SSH key, a one-time code, or a combination of authentication methods.

Users should configure their required authentication methods before relying on remote access.

For instructions on configuring Rostam authentication, see:

[Two-Factor Authentication Setup](/ssh/otp)

> **Important:** SSH public key authentication does not necessarily eliminate two-factor authentication requirements. The authentication methods required for your account are determined by the cluster's security configuration.

## Understanding the Login Node

When you connect to `rostam.cct.lsu.edu`, you are accessing the cluster's login environment.

Login nodes provide a convenient place to:

- Edit source code and scripts.
- Manage files and directories.
- Configure software environments.
- Compile small programs.
- Submit and monitor Slurm jobs.
- Review job output and results.

Login nodes are shared by multiple users and are not intended for computationally intensive workloads.

**CPU-intensive, memory-intensive, and GPU-intensive applications should be executed on compute nodes through Slurm.**

For example, to request a one-hour interactive allocation:

```bash
srun -N 1 -t 1:00:00 --pty /bin/bash -l
```

This requests an interactive shell within a Slurm allocation, subject to partition defaults and available resources.

For more information about the cluster's computing resources, consult the [Rostam Hardware Inventory](/cluster/hardware).

## Transferring Files

SSH supports encrypted file transfers between your local computer and Rostam.

The most common methods are:

- `scp` — Securely copy files and directories.
- `sftp` — Interactive or graphical file transfers.
- `rsync` — Synchronize files and directories efficiently over SSH.

### Using SCP

To copy a file from your local computer to your Rostam home directory:

```bash
scp myfile.txt myuser@rostam.cct.lsu.edu:~/
```

To copy a file from Rostam to your local computer:

```bash
scp myuser@rostam.cct.lsu.edu:~/results.txt .
```

The final `.` means the current local directory.

To recursively copy a directory to Rostam:

```bash
scp -r myproject/ myuser@rostam.cct.lsu.edu:~/
```

These commands can be used from Linux, macOS, and Windows PowerShell when OpenSSH is installed.

### Using SFTP

To start an interactive SFTP session:

```bash
sftp myuser@rostam.cct.lsu.edu
```

Useful commands include:

| Command | Purpose |
|---|---|
| `ls` | List remote files |
| `pwd` | Show current remote directory |
| `lpwd` | Show current local directory |
| `cd` | Change remote directory |
| `lcd` | Change local directory |
| `put file.txt` | Upload a file |
| `get file.txt` | Download a file |
| `mkdir name` | Create a remote directory |
| `bye` | End the SFTP session |

For example:

```text
sftp> put input.dat
sftp> get results.dat
sftp> bye
```

### Graphical File Transfers

Users who prefer a graphical interface can use:

- [WinSCP](https://winscp.net/) — Popular Windows SFTP and SCP client.
- [FileZilla](https://filezilla-project.org/) — Cross-platform file transfer application supporting SFTP.
- [Cyberduck](https://cyberduck.io/) — Graphical file transfer client for Windows and macOS.
- MobaXterm — Integrated SFTP file browser.
- Bitvise SSH Client — Integrated graphical file transfer.

Configure the application to use:

```text
Protocol: SFTP
Host: rostam.cct.lsu.edu
Port: 22
Username: your Rostam username
```

Use the authentication method required for your account.

### Using rsync

For large projects or directories that change frequently, `rsync` can be more efficient than repeatedly copying everything with `scp`.

For example:

```bash
rsync -avh myproject/ \
    myuser@rostam.cct.lsu.edu:~/myproject/
```

To copy results back to your local computer:

```bash
rsync -avh \
    myuser@rostam.cct.lsu.edu:~/results/ \
    ./results/
```

`rsync` generally transfers only the differences between existing source and destination files, making it useful for recurring transfers.

It must be available on both systems.

> **Note:** Large datasets and substantial file transfers should follow the cluster's storage and data transfer policies. Avoid unnecessary transfers through shared login infrastructure.

## Connecting with X11 Forwarding

Some scientific applications require a graphical display.

SSH supports **X11 forwarding**, which allows a graphical application running on a remote Linux system to display its windows on your local computer.

X11 forwarding requires an X server on your workstation and appropriate support on the remote system.

### Linux

Most traditional Linux desktop environments can support X11 applications, although additional configuration may be necessary when using Wayland.

Connect using:

```bash
ssh -X myuser@rostam.cct.lsu.edu
```

Some applications require trusted X11 forwarding:

```bash
ssh -Y myuser@rostam.cct.lsu.edu
```

The `-Y` option provides fewer security restrictions than `-X` and should only be used with trusted remote systems.

### macOS

macOS does not include a built-in X11 server.

Install [XQuartz](https://www.xquartz.org/) to enable X11 support.

After installing XQuartz, start the application and connect using:

```bash
ssh -X myuser@rostam.cct.lsu.edu
```

Some applications may require:

```bash
ssh -Y myuser@rostam.cct.lsu.edu
```

A logout or restart may be necessary after installing XQuartz.

### Windows

MobaXterm includes an integrated X11 server, making it one of the simplest options for graphical SSH sessions on Windows.

Enable its X11 server and X11 forwarding support before connecting.

Other Windows X servers, such as [VcXsrv](https://sourceforge.net/projects/vcxsrv/), may be used with OpenSSH or PuTTY.

PuTTY includes an X11 forwarding option under:

```text
Connection → SSH → X11
```

An X server must be running locally for forwarded applications to display.

### Checking X11 Forwarding

After connecting, check the remote display variable:

```bash
echo $DISPLAY
```

If X11 forwarding is enabled, this normally returns a display identifier.

For example:

```text
localhost:10.0
```

An empty value generally indicates that X11 forwarding has not been established.

> **Important:** Graphical applications can be slow over high-latency connections. X11 forwarding is appropriate for lightweight graphical tools, but demanding visualization workloads may require other remote visualization approaches.

## Advanced SSH Configuration

SSH provides several features that can simplify repeated access to Rostam.

These include:

- Public key authentication.
- SSH client configuration files.
- Connection aliases.
- SSH authentication agents.
- ProxyJump connections.
- SSH port forwarding.

For detailed instructions, see:

[Advanced SSH Configuration](/ssh/advanced_ssh)

### SSH Public Key Authentication

SSH keys provide a cryptographic alternative to password-based authentication.

A key pair consists of:

- A **private key**, which remains on your local computer.
- A **public key**, which may be registered with the remote system.

You can generate an Ed25519 key pair with:

```bash
ssh-keygen -t ed25519
```

SSH keys should be protected with an appropriate passphrase.

Private keys should never be shared.

The public key must be registered using a method permitted by Rostam's account and authentication policies.

### Simplifying Connections with SSH Config

Rather than typing the complete SSH command every time, you can create a local SSH configuration file.

On Linux and macOS, the file is normally:

```text
~/.ssh/config
```

For Windows OpenSSH, the configuration file is normally:

```text
%USERPROFILE%\.ssh\config
```

Add the following entry:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    Port 22
```

You can then connect using:

```bash
ssh rostam
```

The alias also works with SCP:

```bash
scp myfile.txt rostam:~/
```

And SFTP:

```bash
sftp rostam
```

This configuration works with OpenSSH clients on Linux, macOS, and Windows.

### Using ProxyJump

SSH ProxyJump can route a connection through a login or gateway host.

For example, an SSH client configuration can use Rostam as an intermediate system:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser

Host rostam-compute
    HostName example-compute-node
    User myuser
    ProxyJump rostam
```

This configuration is illustrative. Replace the compute node name with an authorized destination.

A connection through ProxyJump does not allocate computational resources or bypass Slurm.

Compute-node access must follow Rostam's scheduling and access policies, and direct SSH access may be restricted.

## Troubleshooting Connections

Most SSH connection problems involve network access, authentication, host key verification, or client configuration.

### Connection Timed Out

A timeout often indicates that the client cannot reach the SSH service.

Possible causes include:

- Network connectivity problems.
- Firewall restrictions.
- VPN or network access requirements.
- An incorrect hostname.
- Temporary server or network unavailability.

Verify that you are using:

```text
rostam.cct.lsu.edu
```

And the default SSH port:

```text
22
```

On Windows PowerShell, you can test basic TCP connectivity:

```powershell
Test-NetConnection rostam.cct.lsu.edu -Port 22
```

On Linux or macOS, an SSH connection attempt with verbose output may help identify the problem:

```bash
ssh -v myuser@rostam.cct.lsu.edu
```

### Permission Denied

An error such as:

```text
Permission denied (publickey,password).
```

indicates that authentication was not accepted using the methods attempted.

Check that:

- Your username is correct.
- Your account is active.
- You are using the appropriate authentication method.
- Required two-factor authentication is configured.
- Any configured SSH key is registered and accessible.

The exact authentication methods listed in the error message may differ.

### Host Key Verification Failed

SSH protects users against unexpected changes in a remote server's identity.

If you see:

```text
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
```

do not automatically remove the stored key or disable host key verification.

A host key change may result from legitimate server maintenance, but it can also indicate a security problem.

Verify the change with the Rostam administrators before accepting a replacement key.

### X11 Applications Do Not Display

If graphical applications fail to open:

1. Confirm that an X server is running locally.
2. Connect using `ssh -X` or, when appropriate, `ssh -Y`.
3. Check that `$DISPLAY` is set.
4. Verify that the application and X11 forwarding dependencies are available remotely.
5. Confirm that the application's resource requirements are appropriate for the allocated node.

### SSH Session Disconnects

Unstable networks, idle connection timeouts, and interrupted internet connections can terminate SSH sessions.

For long-running terminal work, a terminal multiplexer such as `tmux` can help preserve shell sessions when the network connection is interrupted.

For example, where installed:

```bash
tmux new -s mysession
```

To detach from the session without terminating it, press:

```text
Ctrl+B, then D
```

To reconnect to an existing session:

```bash
tmux attach -t mysession
```

Interactive terminal sessions should not be used as a substitute for Slurm batch jobs.

Computational workloads should be submitted through Slurm so that they are managed independently of the user's SSH connection.

## Getting Started After Login

Once connected, several basic Linux commands can help you become familiar with the environment.

| Command | Description |
|---|---|
| `pwd` | Display the current directory |
| `ls` | List files and directories |
| `ls -lah` | Show a detailed directory listing |
| `cd directory` | Change directories |
| `cd ~` | Return to your home directory |
| `mkdir name` | Create a directory |
| `cp source dest` | Copy a file |
| `mv source dest` | Move or rename a file |
| `less file.txt` | View a text file |
| `nano file.txt` | Edit a text file, if nano is installed |
| `module avail` | List available software modules |
| `module list` | Show currently loaded modules |
| `sinfo` | Display Slurm partition and node information |
| `squeue -u $USER` | Display your queued and running jobs |
| `exit` | Disconnect from the system |

### Check Your Account

Display your username:

```bash
whoami
```

Display your current directory:

```bash
pwd
```

Display your home directory:

```bash
echo $HOME
```

### Explore Available Computing Resources

View the current Slurm partition and node summary:

```bash
sinfo
```

Show the status of your jobs:

```bash
squeue -u "$USER"
```

Display available software modules:

```bash
module avail
```

These commands provide a useful starting point for understanding the Rostam environment.

## Additional Documentation

- [Rostam Hardware Inventory](/cluster/hardware)
- [Two-Factor Authentication Setup](/ssh/otp)
- [Advanced SSH Configuration](/ssh/advanced_ssh)
- [OpenSSH](https://www.openssh.com/)
- [OpenSSH Manual Pages](https://www.openssh.com/manual.html)
- [PuTTY](https://www.putty.org/)
- [MobaXterm](https://mobaxterm.mobatek.net/)
- [WinSCP](https://winscp.net/)
- [XQuartz](https://www.xquartz.org/)
- [Slurm Workload Manager](https://slurm.schedmd.com/documentation.html)
