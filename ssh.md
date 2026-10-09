---
title: Connecting to Rostam
description: How to connect to Rostam via ssh
published: true
date: 2026-10-09T19:23:39.571Z
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


## Transferring Files To and From Rostam

Rostam supports secure file transfers using the SSH protocol. Users can upload source code, input datasets, scripts, and other files to the cluster, as well as download computational results and output files to their local computers.

File transfers can be performed using command-line utilities or graphical applications, depending on your operating system and preferences.

All file transfers should use encrypted protocols such as **SFTP, SCP, or rsync over SSH**. Unencrypted FTP is not recommended.

The connection information is:

| Setting | Value |
|---|---|
| Hostname | `rostam.cct.lsu.edu` |
| Port | `22` |
| Protocol | SFTP or SCP |
| Username | Your Rostam username |
| Authentication | Your configured SSH authentication method |

> **Important:** File transfers must comply with Rostam's authentication requirements, including two-factor authentication where applicable. Some graphical clients may require additional configuration to support interactive authentication.

### Choosing a File Transfer Method

Several tools are available for transferring files between your computer and Rostam.

| Tool | Interface | Operating Systems | Recommended Use |
|---|---|---|---|
| SCP | Command line | Linux, macOS, Windows | Simple file and directory copies |
| SFTP | Command line | Linux, macOS, Windows | Interactive file transfers |
| rsync | Command line | Linux, macOS, Windows with WSL or rsync installed | Large directories and repeated synchronization |
| WinSCP | Graphical | Windows | General-purpose file management and transfers |
| MobaXterm | Graphical | Windows | Integrated SSH terminal and file transfers |
| FileZilla | Graphical | Linux, macOS, Windows | Cross-platform drag-and-drop transfers |
| Cyberduck | Graphical | macOS, Windows | Simple graphical file transfers |
| Bitvise SSH Client | Graphical | Windows | Integrated SSH and SFTP |
| Termius | Graphical | Linux, macOS, Windows | SSH session management and supported SFTP workflows |

For occasional transfers, SCP or SFTP is usually sufficient. For users who prefer working with graphical applications, WinSCP and MobaXterm are particularly convenient on Windows.

For recurring transfers involving large directories, rsync is generally more efficient.

## Command-Line File Transfers

Linux and macOS normally provide OpenSSH file transfer tools as part of their standard software environments.

Modern Windows systems can also use these tools through PowerShell or Windows Terminal.

Commands in the following examples are executed **on your local computer**, unless otherwise indicated.

Replace `myuser` with your Rostam username.

### SCP: Secure Copy

The `scp` command securely copies files between local and remote systems.

Its basic syntax is:

```bash
scp source destination
```

A remote location is specified using:

```text
username@hostname:path
```

#### Upload a File to Rostam

To upload `myfile.txt` from your local computer to your Rostam home directory:

```bash
scp myfile.txt myuser@rostam.cct.lsu.edu:~/
```

To place the file in a particular directory:

```bash
scp myfile.txt \
    myuser@rostam.cct.lsu.edu:~/project/
```

The destination directory must already exist.

#### Download a File from Rostam

To download `results.txt` from your Rostam home directory:

```bash
scp myuser@rostam.cct.lsu.edu:~/results.txt .
```

The final `.` refers to the current local directory.

To save it under a different local filename:

```bash
scp myuser@rostam.cct.lsu.edu:~/results.txt \
    local-results.txt
```

#### Transfer an Entire Directory

Use the `-r` option to recursively copy a directory.

Upload a directory:

```bash
scp -r myproject/ \
    myuser@rostam.cct.lsu.edu:~/
```

Download a directory:

```bash
scp -r myuser@rostam.cct.lsu.edu:~/results/ \
    ./results/
```

For directories containing many files, rsync may be a better choice.

#### Transfer Multiple Files

Several files can be uploaded in one command:

```bash
scp input.dat config.txt job.sh \
    myuser@rostam.cct.lsu.edu:~/project/
```

#### SCP on Windows PowerShell

The same commands work in Windows PowerShell when OpenSSH is installed.

For example, upload a file from your Downloads folder:

```powershell
scp "$HOME\Downloads\input.dat" myuser@rostam.cct.lsu.edu:~/
```

Download results to your Desktop:

```powershell
scp myuser@rostam.cct.lsu.edu:~/results.dat "$HOME\Desktop\"
```

Windows paths containing spaces should be enclosed in quotation marks.

### SFTP: Secure File Transfer Protocol

SFTP provides an interactive environment for browsing directories, uploading files, and downloading results.

To connect:

```bash
sftp myuser@rostam.cct.lsu.edu
```

After authenticating, you will see a prompt similar to:

```text
sftp>
```

SFTP supports commands for working with both local and remote directories.

| Command | Description |
|---|---|
| `pwd` | Display current remote directory |
| `lpwd` | Display current local directory |
| `ls` | List remote files |
| `lls` | List local files |
| `cd directory` | Change remote directory |
| `lcd directory` | Change local directory |
| `mkdir directory` | Create remote directory |
| `put file` | Upload a file |
| `get file` | Download a file |
| `put -r directory` | Upload a directory recursively |
| `get -r directory` | Download a directory recursively |
| `bye` | Disconnect |

#### Example SFTP Session

Connect:

```bash
sftp myuser@rostam.cct.lsu.edu
```

Inspect your remote directory:

```text
sftp> pwd
sftp> ls
```

Change the remote working directory:

```text
sftp> cd project
```

Upload an input file:

```text
sftp> put input.dat
```

Download a result file:

```text
sftp> get output.dat
```

Disconnect:

```text
sftp> bye
```

SFTP is useful when several transfers must be performed during one session without repeatedly reconnecting.

### rsync: Efficient Directory Synchronization

The `rsync` utility is useful for transferring large directory structures or synchronizing files that change over time.

Unlike a simple copy operation, rsync can compare source and destination files and avoid retransmitting unchanged data.

This makes it especially useful for research projects involving recurring transfers of input data, source code, and computational results.

#### Upload a Project Directory

```bash
rsync -avh myproject/ \
    myuser@rostam.cct.lsu.edu:~/myproject/
```

The options are:

| Option | Description |
|---|---|
| `-a` | Archive mode, preserving directory structure and file metadata where supported |
| `-v` | Verbose output |
| `-h` | Human-readable sizes |
| `--progress` | Display transfer progress |
| `--partial` | Retain partially transferred files |

For a transfer that may be interrupted:

```bash
rsync -avh --progress --partial \
    myproject/ \
    myuser@rostam.cct.lsu.edu:~/myproject/
```

#### Download Computational Results

```bash
rsync -avh --progress \
    myuser@rostam.cct.lsu.edu:~/results/ \
    ./results/
```

When repeated, rsync generally transfers only files that need updating.

#### Preview Changes Before Transferring

Use `--dry-run` to preview the operation:

```bash
rsync -avhn myproject/ \
    myuser@rostam.cct.lsu.edu:~/myproject/
```

This displays the proposed file operations without actually transferring data.

#### Understanding Trailing Slashes

With rsync, trailing slashes affect how directories are copied.

```bash
rsync -avh myproject/ destination/
```

Copies the **contents** of `myproject` into `destination`.

```bash
rsync -avh myproject destination/
```

Copies the `myproject` directory itself into `destination`.

> **Caution:** rsync supports options such as `--delete` that can remove files from the destination. Users should understand these options and test unfamiliar synchronization commands with `--dry-run` before using them.

#### rsync on Windows

Windows users can run rsync through the Windows Subsystem for Linux (WSL), provided rsync is installed in the Linux environment.

For example, a file in:

```text
C:\Users\myuser\Documents\project
```

may be accessible within WSL as:

```text
/mnt/c/Users/myuser/Documents/project
```

The transfer command would be:

```bash
rsync -avh \
    /mnt/c/Users/myuser/Documents/project/ \
    myuser@rostam.cct.lsu.edu:~/project/
```

WSL provides a useful Linux-compatible environment for Windows users performing command-line file management and software development.

## Graphical File Transfers

Graphical file transfer applications provide a convenient way to browse Rostam directories and transfer files without using command-line utilities.

Most support drag-and-drop operations and provide separate views of your local and remote filesystems.

Use **SFTP over SSH** when configuring these applications.

### WinSCP (Windows)

[WinSCP](https://winscp.net/) is a free Windows file transfer application supporting SFTP and SCP.

It is particularly useful for users who want a graphical file manager for Rostam.

<https://winscp.net/>

#### Connecting with WinSCP

1. Download and install WinSCP.
2. Launch the application.
3. Select **New Site**.
4. Configure the connection:

   - File protocol: `SFTP`
   - Host name: `rostam.cct.lsu.edu`
   - Port number: `22`
   - User name: your Rostam username

5. Click **Login**.
6. Verify the server's SSH host key fingerprint when prompted.
7. Complete the required authentication steps.

WinSCP may allow the connection to be saved as a site profile for future use.

#### Transferring Files

WinSCP can display local and remote directories side by side.

Files can be uploaded or downloaded by:

- Dragging files between directories.
- Using the Upload or Download commands.
- Copying multiple selected files.
- Synchronizing directories.

WinSCP also supports resume and transfer queue features for supported operations.

#### Editing Remote Files

WinSCP includes file editing capabilities.

A remote text file can be opened in an editor, modified, and saved back to Rostam.

This is convenient for editing small configuration files and Slurm job scripts.

For larger software projects, a dedicated development environment may be more appropriate.

### MobaXterm (Windows)

[MobaXterm](https://mobaxterm.mobatek.net/) combines SSH terminal access with an integrated graphical SFTP browser.

For many Windows HPC users, it provides a convenient all-in-one environment.

#### Connecting with MobaXterm

1. Open MobaXterm.
2. Select **Session**.
3. Choose **SSH**.
4. Enter `rostam.cct.lsu.edu`.
5. Specify your Rostam username.
6. Leave the SSH port at `22`.
7. Open the session and complete authentication.

#### Using the SFTP Browser

When an SSH session is established, MobaXterm can display a remote SFTP file browser beside the terminal.

This allows users to:

- Browse directories on Rostam.
- Upload local files.
- Download remote files.
- Create and rename directories.
- Edit supported remote text files.
- Transfer files using drag-and-drop.

The remote file browser operates over SFTP, while the terminal provides command-line access to the same system.

A separate file transfer connection may be necessary depending on client configuration and authentication behavior.

#### Advantages for HPC Users

MobaXterm provides several capabilities in a single application:

- SSH connections.
- SFTP file transfers.
- Multiple terminal sessions.
- X11 forwarding.
- SSH key management.
- Saved connection profiles.

It is a convenient option for users who regularly work with both terminal commands and graphical file transfers.

### FileZilla (Linux, macOS, Windows)

[FileZilla](https://filezilla-project.org/) is a cross-platform graphical file transfer application supporting SFTP.

It is a useful option for users who work on different operating systems and prefer the same interface on each.

#### Connecting with FileZilla

1. Install FileZilla Client.
2. Open **File → Site Manager**.
3. Create a new site.
4. Configure:

   - Protocol: `SFTP - SSH File Transfer Protocol`
   - Host: `rostam.cct.lsu.edu`
   - Port: `22`
   - Logon Type: an authentication method supported by your Rostam account
   - User: your Rostam username

5. Connect and verify the server's host key.
6. Complete authentication.

Some authentication workflows, particularly those involving two-factor authentication, may require additional configuration or may not be supported by every FileZilla version.

#### Transferring Files

FileZilla provides separate local and remote directory browsers.

Users can:

- Drag files between systems.
- Queue multiple transfers.
- Transfer directories recursively.
- View transfer progress.
- Resume supported interrupted transfers.
- Manage remote directories.

FileZilla is useful for occasional and moderately sized file transfers.

### Cyberduck (macOS and Windows)

[Cyberduck](https://cyberduck.io/) is a graphical file transfer application for macOS and Windows.

It provides a straightforward interface for working with remote files.

#### Connecting with Cyberduck

1. Install and open Cyberduck.
2. Select **Open Connection**.
3. Choose **SFTP (SSH File Transfer Protocol)**.
4. Enter:

   - Server: `rostam.cct.lsu.edu`
   - Port: `22`
   - Username: your Rostam username

5. Connect and verify the server identity.
6. Complete authentication.

Cyberduck supports bookmarks for frequently used connections.

#### Transferring Files

Users can upload or download files using the graphical browser.

Cyberduck also supports remote file editing through compatible local editors and provides transfer progress information.

It is particularly convenient for macOS users who prefer a native graphical application.

### Other Graphical Clients

Additional file transfer applications include:

| Application | Platform | Description |
|---|---|---|
| [Bitvise SSH Client](https://www.bitvise.com/ssh-client) | Windows | Integrated SSH terminal and graphical SFTP |
| [SecureFX](https://www.vandyke.com/products/securefx/) | Windows, macOS, Linux | Commercial file transfer client with advanced session management |
| [Termius](https://termius.com/) | Multiple platforms | SSH client with file transfer features on supported plans and platforms |
| [Transmit](https://panic.com/transmit/) | macOS | Commercial file transfer application with SFTP support |

Users may choose whichever client best suits their operating system and workflow, provided it supports Rostam's SSH authentication requirements.

## File Transfers with Visual Studio Code

[Visual Studio Code](https://code.visualstudio.com/) provides a remote development environment through its **Remote - SSH** extension.

Rather than manually copying files back and forth, users can open and edit files on Rostam through a local VS Code window.

This can be useful for:

- Editing source code.
- Managing Slurm job scripts.
- Browsing project directories.
- Working with version-controlled projects.
- Performing lightweight remote development.

The extension is available for Windows, macOS, and Linux.

### Connecting with Remote - SSH

1. Install Visual Studio Code.
2. Install the **Remote - SSH** extension.
3. Open the Command Palette.
4. Select **Remote-SSH: Connect to Host**.
5. Enter:

```text
myuser@rostam.cct.lsu.edu
```

6. Complete SSH authentication.

Remote - SSH requires a compatible remote environment and installs remote-side components. Its use may be subject to cluster policies and software compatibility.

VS Code remote development should not be used to run resource-intensive computational workloads on login nodes.

Such work must be submitted through Slurm.

## Large Datasets and Long-Running Transfers

Scientific applications may generate substantial amounts of output, including simulation data, checkpoint files, and visualization products.

For large transfers, several considerations can improve reliability and performance.

### Prefer rsync for Repeated Transfers

When copying a directory that is updated regularly, rsync can avoid retransmitting unchanged files.

For example:

```bash
rsync -avh --progress --partial \
    myuser@rostam.cct.lsu.edu:~/simulation-output/ \
    ./simulation-output/
```

If a transfer is interrupted, running the command again can avoid retransferring files that are already complete.

### Consider File Counts

Directories containing hundreds of thousands of small files can be expensive to traverse and transfer.

For large collections of small files, creating an archive may reduce filesystem overhead.

For example, on Rostam:

```bash
tar -czf results.tar.gz results/
```

Then download the archive from your local computer:

```bash
scp myuser@rostam.cct.lsu.edu:~/results.tar.gz .
```

Extract it locally:

```bash
tar -xzf results.tar.gz
```

Compression is not always beneficial, particularly for data that is already compressed or difficult to compress.

### Avoid Unnecessary Transfers

For large datasets, consider whether the data can be analyzed or reduced on Rostam before transferring it.

For example, it may be more efficient to download selected results, summaries, or derived products rather than copying every intermediate output file.

### Use Appropriate Storage Locations

Rostam may provide different storage locations for home directories, applications, and computational data.

Users should consult the cluster's storage documentation before placing large datasets in their home directories.

Temporary and scratch storage may have different retention and backup policies.

> **Important:** Large transfers and intensive filesystem operations should comply with cluster policies. Dedicated data-transfer services, if provided, should be preferred for workloads that would otherwise place significant load on the login nodes.

## File Transfer Troubleshooting

### Permission Denied

If a transfer reports:

```text
Permission denied
```

Check that:

- Your Rostam account is active.
- The username is correct.
- Authentication completed successfully.
- You have permission to read the source file.
- You have permission to write to the destination directory.

Linux file permissions can be inspected with:

```bash
ls -l filename
```

Directory permissions can be examined using:

```bash
ls -ld directory
```

### No Such File or Directory

An error such as:

```text
No such file or directory
```

usually indicates that a source or destination path is incorrect.

Remember that local and remote paths are different.

For example:

```bash
scp myuser@rostam.cct.lsu.edu:~/results.txt .
```

downloads a remote file, while:

```bash
scp results.txt myuser@rostam.cct.lsu.edu:~/
```

uploads a local file.

### Connection Timed Out

If the client cannot connect, verify:

- Hostname: `rostam.cct.lsu.edu`
- Port: `22`
- Protocol: SFTP or SCP
- Network connectivity
- Any VPN or institutional network requirements

A connection timeout is different from an authentication failure.

### Two-Factor Authentication Problems

Some graphical applications do not handle interactive authentication in the same manner as a terminal-based SSH client.

If an application fails to complete authentication:

1. Verify that ordinary SSH access works.
2. Confirm that the file transfer application supports the required authentication methods.
3. Check whether the application supports keyboard-interactive authentication.
4. Try a current version of the client.
5. Consult the [Two-Factor Authentication Setup](/ssh/otp) documentation.

Do not disable host key verification or two-factor authentication as a workaround.

### Interrupted Transfers

For recurring or interrupted transfers, rsync is generally preferable to repeatedly copying an entire directory.

For graphical clients, check whether the application supports resuming transfers and preserving partially downloaded files.

### Disk Space and Quotas

Transfers may fail if the destination filesystem is full or a storage quota has been reached.

On Rostam, available space can be inspected with:

```bash
df -h
```

This reports filesystem capacity but may not reflect individual user quotas.

Check storage documentation for applicable quota commands and policies.

## File Transfer Security

File transfers should follow the same security practices as interactive SSH access.

- Use encrypted transfer protocols.
- Verify SSH host keys before accepting them.
- Protect private SSH keys with appropriate permissions and passphrases.
- Do not share passwords or one-time authentication codes.
- Avoid storing credentials in scripts or unprotected configuration files.
- Use trusted software obtained from official sources.
- Disconnect sessions when they are no longer needed.

SSH authentication and file transfer authentication are related, but individual applications may support different methods and features.

## Additional File Transfer Resources

- [OpenSSH](https://www.openssh.com/)
- [OpenSSH SCP Manual](https://man.openbsd.org/scp)
- [OpenSSH SFTP Manual](https://man.openbsd.org/sftp)
- [rsync Documentation](https://rsync.samba.org/documentation.html)
- [WinSCP](https://winscp.net/)
- [MobaXterm](https://mobaxterm.mobatek.net/)
- [FileZilla](https://filezilla-project.org/)
- [Cyberduck](https://cyberduck.io/)
- [Bitvise SSH Client](https://www.bitvise.com/ssh-client)
- [Visual Studio Code Remote - SSH](https://code.visualstudio.com/docs/remote/ssh)
- [Two-Factor Authentication Setup](/ssh/otp)
- [Advanced SSH Configuration](/ssh/advanced_ssh)

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
