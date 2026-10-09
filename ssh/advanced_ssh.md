---
title: Advaced SSH Configuration
description: 
published: true
date: 2026-10-09T22:16:24.052Z
tags: 
editor: markdown
dateCreated: 2023-07-13T17:39:55.721Z
---


# Advanced SSH Configuration

SSH provides several features that make working with Rostam more convenient, secure, and efficient. With a properly configured SSH client, users can simplify authentication, create connection shortcuts, maintain interactive sessions, and access allocated compute nodes through the Rostam login system.

This guide covers:

- SSH public key authentication
- Generating and installing SSH keys
- Managing keys with SSH agents
- Configuring SSH connection aliases
- Keeping SSH connections alive
- Accessing compute nodes with ProxyJump
- Using SSH with graphical applications and file transfers
- Troubleshooting common SSH configuration problems

These features are available through OpenSSH on Linux, macOS, and Windows. Graphical clients such as PuTTY, MobaXterm, and other SSH applications provide equivalent capabilities.

For basic login instructions, see [Accessing Rostam](/ssh).

## SSH Public Key Authentication

SSH public key authentication uses cryptographic keys to establish your identity when connecting to a remote system.

Instead of relying exclusively on a password, your computer uses a private key to prove that it possesses the credentials associated with a public key registered on the remote system.

An SSH key pair consists of two files:

| Key | Purpose |
|---|---|
| Private key | Remains on your local computer and must be protected |
| Public key | Can be installed on systems where SSH key authentication is permitted |

The private key must never be shared. The public key can be distributed to systems on which you are authorized to log in.

### SSH Keys and Two-Factor Authentication

SSH key authentication can eliminate the need to enter an account password for every connection when permitted by the server's authentication policies.

However, SSH keys do not necessarily eliminate other authentication requirements.

Rostam uses two-factor authentication (2FA), and users may still be required to provide a one-time authentication code or complete another verification step.

SSH keys simplify the authentication process; they do not bypass university security requirements.

See [Two-Factor Authentication Setup](/ssh/otp) for additional information.

## Generating a New SSH Key

Modern OpenSSH clients support several cryptographic key types.

**Ed25519 is recommended for new SSH keys** because it provides strong security with small keys and efficient authentication.

RSA keys remain supported for compatibility with older systems and applications.

### Linux and macOS

Open a terminal on your local computer.

Generate a new Ed25519 key pair:

```bash
ssh-keygen -t ed25519 -C "rostam"
```

You will be prompted for a location in which to save the key.

Example:

```text
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/myuser/.ssh/id_ed25519):
```

Press Enter to accept the default location, provided you do not already have a key stored there that you wish to preserve.

The default files are:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

The first is the private key. The second is the public key.

You will then be prompted for a passphrase:

```text
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

A passphrase protects the private key if someone gains access to the file.

**Using a passphrase is strongly recommended.** An SSH agent can remember your unlocked key so that you do not need to enter the passphrase on every connection.

A successful key generation operation produces output similar to:

```text
Your identification has been saved in /home/myuser/.ssh/id_ed25519
Your public key has been saved in /home/myuser/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:... rostam
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|    ...          |
|       ...       |
|       ...       |
|                 |
+----[SHA256]-----+
```

The fingerprint and randomart will be unique to your key.

### Windows PowerShell

Windows 10 and Windows 11 include an OpenSSH client on many installations.

Open PowerShell or Windows Terminal and generate a key:

```powershell
ssh-keygen -t ed25519 -C "rostam"
```

The default key location is:

```text
C:\Users\YourUsername\.ssh\id_ed25519
```

The corresponding public key is:

```text
C:\Users\YourUsername\.ssh\id_ed25519.pub
```

The key generation and passphrase prompts work the same way as on Linux and macOS.

If Windows reports that `ssh-keygen` is not recognized, install the optional Windows OpenSSH Client feature.

### Using RSA Keys

Ed25519 is preferred for new installations, but some older SSH clients may require RSA keys.

To generate a 4096-bit RSA key:

```bash
ssh-keygen -t rsa -b 4096 -C "rostam"
```

The default filenames are:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

Existing RSA keys do not necessarily need to be replaced if they use appropriate algorithms, remain secure, and are accepted by the server.

## Installing Your Public Key on Rostam

After generating an SSH key pair, the public key must be registered with the remote system.

On systems that permit user-managed SSH authorized keys, the public key is placed in:

```text
~/.ssh/authorized_keys
```

The server uses this file to determine which public keys are authorized for the account.

### Using ssh-copy-id

On Linux systems with `ssh-copy-id` installed, the simplest method is:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub \
    myuser@rostam.cct.lsu.edu
```

Replace `myuser` with your Rostam username.

The command connects to Rostam using your existing authentication method and attempts to install the public key.

After successful installation, test the key:

```bash
ssh -i ~/.ssh/id_ed25519 \
    myuser@rostam.cct.lsu.edu
```

You may still receive a two-factor authentication prompt.

### macOS

Some macOS installations do not include `ssh-copy-id`.

If you use Homebrew, you can install it with:

```bash
brew install ssh-copy-id
```

Then run:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub \
    myuser@rostam.cct.lsu.edu
```

### Installing a Key Manually

If `ssh-copy-id` is unavailable, the public key can be installed manually, provided Rostam permits user-managed authorized keys.

On Linux or macOS:

```bash
cat ~/.ssh/id_ed25519.pub | \
ssh myuser@rostam.cct.lsu.edu \
'umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys'
```

This creates the remote `.ssh` directory if necessary and appends the public key to `authorized_keys`.

The command does not overwrite existing keys.

For an existing authorized keys file, permissions can be verified after logging into Rostam:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

### Installing a Public Key from Windows

Windows PowerShell does not ordinarily include `ssh-copy-id`, but the public key can be displayed with:

```powershell
Get-Content "$HOME\.ssh\id_ed25519.pub"
```

Copy the complete public key, beginning with `ssh-ed25519`.

After connecting to Rostam using your existing authentication method, create the SSH directory if necessary:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Edit the authorized keys file:

```bash
nano ~/.ssh/authorized_keys
```

Paste the public key on a single line, save the file, and set its permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

If `nano` is unavailable, use another installed text editor such as `vi` or `vim`.

Alternatively, Windows users with OpenSSH may install the key using a PowerShell pipeline:

```powershell
Get-Content "$HOME\.ssh\id_ed25519.pub" |
    ssh myuser@rostam.cct.lsu.edu 'umask 077; mkdir -p ~/.ssh; cat >> ~/.ssh/authorized_keys'
```

### Testing Public Key Authentication

From your local computer:

```bash
ssh -i ~/.ssh/id_ed25519 \
    myuser@rostam.cct.lsu.edu
```

For additional authentication details:

```bash
ssh -v -i ~/.ssh/id_ed25519 \
    myuser@rostam.cct.lsu.edu
```

The verbose output can help identify whether the client offered the intended key and whether the server accepted it.

If Rostam uses centralized SSH key management or restricts user-managed authorized keys, public keys must be registered through the approved account-management process instead.

## Managing SSH Keys with an SSH Agent

A passphrase protects your private key, but entering it repeatedly can become inconvenient.

An SSH agent keeps unlocked private keys available in memory so that subsequent SSH connections can use them without requiring the passphrase every time.

This allows you to retain the security benefits of a passphrase while simplifying everyday access.

### Linux

Many Linux desktop environments start an SSH agent automatically.

Check whether an agent is available:

```bash
ssh-add -l
```

If an agent is running but contains no keys, you may see:

```text
The agent has no identities.
```

Add your private key:

```bash
ssh-add ~/.ssh/id_ed25519
```

You will be prompted for the key's passphrase.

If no SSH agent is available, you can start one for your current shell:

```bash
eval "$(ssh-agent -s)"
```

Then add your key:

```bash
ssh-add ~/.ssh/id_ed25519
```

The unlocked key remains available while the agent retains it.

### macOS

macOS supports SSH agents and integration with the system Keychain.

To add a key and store its passphrase in the Keychain:

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

The SSH configuration can also use:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519
    AddKeysToAgent yes
    UseKeychain yes
```

`UseKeychain` is specific to Apple's OpenSSH implementation and should not be copied into a standard Linux or Windows configuration unless supported by that client.

### Windows

Windows provides an OpenSSH Authentication Agent service.

Open PowerShell with administrator privileges to enable the service:

```powershell
Set-Service -Name ssh-agent -StartupType Automatic
Start-Service ssh-agent
```

Then, in your normal PowerShell session, add your private key:

```powershell
ssh-add "$HOME\.ssh\id_ed25519"
```

List loaded keys:

```powershell
ssh-add -l
```

An SSH agent reduces repeated passphrase prompts but does not eliminate Rostam's required authentication policies.

## Using the SSH Configuration File

OpenSSH supports a client-side configuration file that stores frequently used connection settings.

Instead of entering a complete hostname, username, port, and identity file for every connection, you can define a short connection alias.

The SSH client configuration file is named:

```text
config
```

It is stored in your local `.ssh` directory.

### Configuration File Location

On Linux and macOS:

```text
~/.ssh/config
```

On Windows:

```text
C:\Users\YourUsername\.ssh\config
```

Or, in PowerShell:

```powershell
$HOME\.ssh\config
```

### Creating the File on Linux or macOS

Create the directory if it does not exist:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

Create the configuration file:

```bash
touch ~/.ssh/config
chmod 600 ~/.ssh/config
```

Open it with your preferred editor:

```bash
nano ~/.ssh/config
```

You may also use `vim`, `vi`, or another text editor.

### Creating the File on Windows

Open PowerShell:

```powershell
New-Item -ItemType Directory -Force "$HOME\.ssh"
```

Then open or create the configuration file using Notepad:

```powershell
notepad "$HOME\.ssh\config"
```

Ensure the filename is `config`, without a `.txt` extension.

Windows manages file permissions through access control lists rather than Unix permission modes.

### Example Rostam Configuration

Add the following to your SSH configuration file:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    Port 22
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
```

The directives have the following meanings:

| Directive | Purpose |
|---|---|
| `Host` | Defines a local connection alias |
| `HostName` | Specifies the actual server hostname |
| `User` | Specifies the remote username |
| `Port` | Specifies the SSH port |
| `IdentityFile` | Selects the private SSH key |
| `IdentitiesOnly` | Limits authentication to explicitly configured identities |

Save the file.

Now connect using:

```bash
ssh rostam
```

Instead of:

```bash
ssh myuser@rostam.cct.lsu.edu
```

SSH reads the connection details from your configuration file automatically.

The same alias can be used for file transfers:

```bash
scp myfile.txt rostam:~/
```

Or:

```bash
sftp rostam
```

### Multiple SSH Connections

The configuration file can contain multiple host definitions.

For example:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519

Host another-system
    HostName another.example.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519
```

You can then connect using:

```bash
ssh rostam
```

Or:

```bash
ssh another-system
```

This is particularly useful for users who work with multiple HPC clusters or research systems.

### Global SSH Client Options

Settings that should apply to every SSH connection can be placed under:

```sshconfig
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

More specific host settings should generally appear before the global `Host *` entry.

## Keeping SSH Connections Alive

Some firewalls, routers, and network address translation (NAT) systems close connections that appear idle for extended periods.

This may result in an SSH session becoming unresponsive even though the remote system is still running.

OpenSSH provides client-side keepalive options to help prevent idle connection timeouts.

Add the following to your Rostam configuration:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

These options mean:

- `ServerAliveInterval 60`: Send an encrypted keepalive request after 60 seconds without receiving data from the server.
- `ServerAliveCountMax 3`: Disconnect if three consecutive keepalive requests receive no response.

Keepalive messages help maintain idle connections and detect broken connections.

They do not guarantee that a session will survive network interruptions, server maintenance, or other disconnections.

### Using tmux for Persistent Terminal Sessions

For interactive work that should survive an interrupted SSH connection, consider using a terminal multiplexer such as `tmux`.

On Rostam, start a session:

```bash
tmux new -s work
```

To detach while leaving the session running, press:

```text
Ctrl+B, then D
```

Later, reconnect to Rostam:

```bash
ssh rostam
```

Then reattach:

```bash
tmux attach -t work
```

A terminal multiplexer preserves interactive shell sessions but does not replace Slurm for computational workloads.

Long-running calculations should be submitted through Slurm so they are not dependent on an interactive login session.

## Connecting Directly to Compute Nodes with ProxyJump

Rostam compute nodes operate primarily on internal cluster networks and are generally not directly reachable from users' workstations.

The Rostam login environment provides a controlled entry point to those internal systems.

OpenSSH includes a feature called **ProxyJump**, which permits an SSH connection to a destination host through an intermediate SSH server.

This allows users to connect from their workstation to an authorized compute node without manually starting a second SSH command on the login node.

### How ProxyJump Works

A normal SSH connection follows this path:

```text
Your Workstation
       |
       | SSH
       v
Rostam Login Node
```

A ProxyJump connection follows this path:

```text
Your Workstation
       |
       | SSH
       v
Rostam Login Node
       |
       | SSH forwarding
       v
Allocated Compute Node
```

The login node acts as an SSH jump host.

ProxyJump does not bypass authentication or resource scheduling.

### Compute Node Access and Slurm

Rostam uses Slurm to allocate computing resources.

Compute-node SSH access may be restricted by `pam_slurm_adopt`, which checks whether the connecting user has an active job allocation on the target node.

For example, attempting to access a compute node without an allocation may produce:

```text
Access denied by pam_slurm_adopt: you have no active jobs on this node
```

This is expected behavior.

Users must first obtain a valid Slurm allocation on the target compute node.

The SSH connection can then be associated with the existing allocation, subject to the node's access policies.

**ProxyJump is a convenience for reaching allocated resources, not an alternative to Slurm scheduling.**

### Example: Configuring Access to Bahram

The following example uses `bahram`, an NVIDIA V100 GPU node in Rostam's `cuda-V100` partition.

Open your local SSH configuration file and add:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    Port 22
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    ServerAliveInterval 60
    ServerAliveCountMax 3

Host bahram
    HostName bahram.rostam.cct.lsu.edu
    User myuser
    Port 22
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    ProxyJump rostam
```

The `ProxyJump rostam` directive tells OpenSSH to establish the connection through the previously defined Rostam host.

If the compute node's internal DNS name differs from the example, use the hostname recognized within the cluster.

### Step 1: Test the Connection

From your workstation:

```bash
ssh bahram
```

Without an active Slurm allocation, you may see:

```text
Access denied by pam_slurm_adopt: you have no active jobs on this node
```

This confirms that the connection reached the compute node but was denied by its allocation-based access controls.

### Step 2: Request a Slurm Allocation

Connect to Rostam:

```bash
ssh rostam
```

Request an allocation on Bahram:

```bash
salloc -p cuda-V100 \
    -N 1 \
    --nodelist=bahram \
    --gres=gpu:1 \
    -t 01:00:00
```

This requests:

- The `cuda-V100` partition.
- One compute node.
- Specifically, the `bahram` node.
- One GPU.
- A maximum allocation of one hour.

The allocation will begin when Slurm determines that the requested resources are available.

The shell remains associated with the allocation until it is released.

### Step 3: Verify the Allocation

After Slurm grants the allocation, inspect it:

```bash
echo "$SLURM_JOB_ID"
```

You can also inspect the assigned nodes:

```bash
squeue -j "$SLURM_JOB_ID" -o "%.18i %.8T %.20R"
```

Or:

```bash
scontrol show job "$SLURM_JOB_ID"
```

### Step 4: Connect from Your Workstation

Leave the Slurm allocation active.

Open a second terminal on your local workstation and enter:

```bash
ssh bahram
```

OpenSSH will connect through the Rostam login node and attempt to establish an SSH session on Bahram.

If your authentication and Slurm allocation permit the connection, you will receive a shell on the compute node.

Check your location:

```bash
hostname
```

Expected output:

```text
bahram
```

The exact hostname presentation may vary.

### Step 5: Release the Allocation

When finished, disconnect from Bahram:

```bash
exit
```

Return to the terminal containing your `salloc` session and exit:

```bash
exit
```

This releases the interactive allocation.

You can confirm that the allocation is no longer active:

```bash
squeue -u "$USER"
```

### Using srun Instead

For many interactive workloads, it is simpler to start a shell directly through Slurm:

```bash
srun -p cuda-V100 \
    -N 1 \
    --gres=gpu:1 \
    -t 01:00:00 \
    --pty /bin/bash -l
```

This allows Slurm to choose an eligible node and start an interactive shell there.

For ordinary command-line work, this is often preferable to connecting separately with SSH.

ProxyJump is especially useful when an application requires a direct SSH connection, such as a supported remote development tool.

## Configuring Multiple Compute Node Aliases

Users who regularly work on different nodes can add SSH aliases for frequently used systems.

For example:

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

Host nasrin0
    HostName nasrin0.rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519
    ProxyJump rostam

Host toranj0
    HostName toranj0.rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519
    ProxyJump rostam
```

These aliases simplify connection commands:

```bash
ssh bahram
ssh nasrin0
ssh toranj0
```

All compute-node connections remain subject to Slurm allocation and access controls.

### Using a Wildcard Configuration

More experienced users can use an SSH wildcard to avoid creating a separate entry for every node.

For example:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519

Host *.rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519
    ProxyJump rostam
```

This allows connections such as:

```bash
ssh bahram.rostam.cct.lsu.edu
```

Or:

```bash
ssh nasrin0.rostam.cct.lsu.edu
```

The `rostam` alias provides the login connection, while the wildcard entry routes matching compute-node hostnames through the login system.

The order of entries matters because OpenSSH generally uses the first value obtained for a configuration parameter.

Wildcard rules should be written carefully to avoid unintentionally applying ProxyJump to unrelated hosts.

## ProxyJump on Windows

Windows OpenSSH supports ProxyJump through PowerShell and Windows Terminal.

The SSH configuration syntax is the same as on Linux and macOS.

Create or edit:

```text
C:\Users\YourUsername\.ssh\config
```

Add:

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

Then connect using PowerShell:

```powershell
ssh bahram
```

The connection is routed through Rostam automatically.

An active allocation on Bahram is still required.

### Using PuTTY

[PuTTY](https://www.putty.org/) provides graphical management of SSH connections.

PuTTY can use an intermediate SSH host through its proxy configuration.

Recent PuTTY versions also support ProxyJump-style connection workflows through proxy settings or appropriately configured SSH connections.

The exact configuration depends on the installed version.

Users should configure:

- The Rostam login host as the intermediate SSH server.
- The compute node as the final destination.
- The appropriate username and authentication method.
- Any required SSH key.
- Host key verification for both systems.

PuTTYgen can generate and convert SSH keys for use with PuTTY. Recent versions support modern key types such as Ed25519.

### Using MobaXterm

[MobaXterm](https://mobaxterm.mobatek.net/) supports SSH gateways.

To configure a compute-node connection:

1. Open MobaXterm.
2. Create a new SSH session.
3. Enter the compute node hostname.
4. Specify your Rostam username.
5. Open the SSH gateway or jump-host configuration.
6. Specify `rostam.cct.lsu.edu` as the gateway.
7. Configure authentication as required.
8. Save the session.

MobaXterm will establish the connection through the login host.

An active Slurm allocation is still required before compute-node access is permitted.

## SSH Port Forwarding

SSH can securely forward network connections between your local computer and remote systems.

This can be useful for applications that expose web interfaces, debugging ports, or other network services.

For example, to forward local port `8888` to a service running on an allocated compute node:

```bash
ssh -N \
    -J rostam \
    -L 8888:localhost:8888 \
    bahram.rostam.cct.lsu.edu
```

This establishes an SSH connection through Rostam to Bahram and forwards local TCP port `8888` to port `8888` on Bahram.

The `-N` option tells SSH not to execute a remote shell command.

After establishing the tunnel, an application listening on the specified port on Bahram may be accessible locally through:

```text
http://localhost:8888
```

This example assumes:

- You have an active Slurm allocation on Bahram.
- SSH access to Bahram is permitted.
- The application is running on Bahram.
- The application is listening on port `8888`.
- The chosen local port is not already in use.

Port forwarding does not provide a Slurm allocation or authorize access to otherwise restricted services.

Users should follow cluster security and resource usage policies when exposing application interfaces.

## Using SSH Configuration with File Transfers

SSH connection aliases can also simplify file transfers.

For example, with this configuration:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519
```

A file can be uploaded using:

```bash
scp input.dat rostam:~/
```

A file can be downloaded using:

```bash
scp rostam:~/results.dat .
```

An SFTP session can be started with:

```bash
sftp rostam
```

For file synchronization:

```bash
rsync -avh myproject/ rostam:~/myproject/
```

Graphical file transfer applications may provide equivalent ways to save connection profiles and SSH identity files.

For detailed file transfer instructions, see the Rostam file transfer documentation.

## SSH Troubleshooting

### SSH Key Is Not Accepted

If authentication fails with a message such as:

```text
Permission denied (publickey,password).
```

Verify:

1. You are using the correct Rostam username.
2. Your public key is registered using an approved method.
3. Your SSH client is using the correct private key.
4. File permissions are correct.
5. The key type is accepted by the server.
6. All required authentication factors have been completed.

Use verbose mode for troubleshooting:

```bash
ssh -v rostam
```

For more detailed diagnostics:

```bash
ssh -vvv rostam
```

Verbose output can reveal which keys are being offered and how authentication proceeds.

### Incorrect Private Key Permissions

On Linux and macOS, OpenSSH may reject private keys that are accessible to other users.

Set appropriate permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 600 ~/.ssh/config
```

The public key can normally be readable by other users:

```bash
chmod 644 ~/.ssh/id_ed25519.pub
```

Windows uses access control lists, and file permissions are managed differently.

### Too Many Authentication Failures

If an SSH agent contains many private keys, the server may reject the connection before the correct key is tried.

Use:

```sshconfig
Host rostam
    HostName rostam.cct.lsu.edu
    User myuser
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
```

This tells the client to limit the identities offered for the connection.

### Compute Node Access Denied

If you receive:

```text
Access denied by pam_slurm_adopt: you have no active jobs on this node
```

Check that you have an active Slurm allocation:

```bash
squeue -u "$USER"
```

Verify that the allocation includes the compute node you are attempting to access.

Having a job somewhere on Rostam does not necessarily authorize SSH access to every compute node.

The allocation must include the target system, and additional node access policies may apply.

### ProxyJump Connection Fails

If a ProxyJump connection fails:

1. Verify that you can SSH to the Rostam login host.
2. Confirm that the target hostname is correct.
3. Verify the SSH configuration file.
4. Check whether your Slurm allocation includes the target node.
5. Confirm that the target node accepts SSH connections.
6. Test with verbose output.

For example:

```bash
ssh -v bahram
```

You can also test ProxyJump without a saved configuration:

```bash
ssh -J myuser@rostam.cct.lsu.edu \
    myuser@bahram.rostam.cct.lsu.edu
```

### SSH Host Key Has Changed

SSH may display:

```text
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
```

This warning indicates that the remote server's identity no longer matches the key previously recorded by your computer.

A change may result from legitimate system maintenance, but it can also indicate a security problem.

Do not automatically remove the old key or disable host key verification.

Verify the new fingerprint with the Rostam administrators before accepting the replacement key.

### Inspecting Effective SSH Configuration

OpenSSH can display the configuration it will use for a particular host.

For example:

```bash
ssh -G rostam
```

To inspect selected options:

```bash
ssh -G rostam | grep -E \
'^(hostname|user|port|identityfile|proxyjump|serveraliveinterval) '
```

For a compute node:

```bash
ssh -G bahram
```

This is useful when multiple SSH configuration entries or wildcard rules may apply.

## Security Recommendations

SSH keys and advanced SSH options can make remote access more convenient, but users should maintain good security practices.

- Prefer Ed25519 keys for new installations.
- Protect private keys with strong passphrases.
- Never share private keys.
- Use an SSH agent rather than removing passphrases for convenience.
- Verify server host keys before accepting them.
- Do not disable two-factor authentication requirements.
- Avoid copying private keys onto remote systems.
- Disable SSH agent forwarding unless it is specifically required and understood.
- Use ProxyJump rather than exposing compute nodes directly to external networks.
- Keep SSH clients and operating systems updated.
- Remove obsolete public keys when they are no longer needed.
- Submit computational work through Slurm rather than running it on shared login nodes.

## Additional Documentation

- [Accessing Rostam](/ssh)
- [Two-Factor Authentication Setup](/ssh/otp)
- [Rostam Hardware Inventory](/cluster/hardware)
- [OpenSSH Documentation](https://www.openssh.com/manual.html)
- [OpenSSH ssh_config Manual](https://man.openbsd.org/ssh_config)
- [OpenSSH ssh-keygen Manual](https://man.openbsd.org/ssh-keygen)
- [OpenSSH ssh-agent Manual](https://man.openbsd.org/ssh-agent)
- [PuTTY](https://www.putty.org/)
- [MobaXterm](https://mobaxterm.mobatek.net/)
- [Slurm Workload Manager](https://slurm.schedmd.com/documentation.html)
- [Slurm pam_slurm_adopt](https://slurm.schedmd.com/pam_slurm_adopt.html)
