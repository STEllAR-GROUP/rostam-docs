---
title: Advaced SSH Configuration
description: 
published: true
date: 2020-03-23T21:23:36.491Z
tags: 
---

# SSH Public Key Authentication

Public key authentication is a way of logging into an SSH/SFTP account using a cryptographic key rather than a password.

Generating a key pair provides you with two long string of characters: a public and a private key. You can place the public key on any server, and then unlock it by connecting to it with a client that already has the private key. When the two match up, the system unlocks without the need for a password. You can increase security even more by protecting the private key with a passphrase.

If you don't already have an SSH key, you must generate a new SSH key.

## Generating a new SSH key

- Open a terminal on your workstation
- Create the RSA key pair

```bash
$ ssh-keygen -t rsa -b 4096
```

- Store the keys and passphrase

Once you have entered the Gen Key command, you will get a few more questions:

```bash
Enter file in which to save the key (/home/tiger/.ssh/id_rsa):
```

You can press enter here, saving the file to the user home (in this case, my example user is called tiger).

```bash
Enter passphrase (empty for no passphrase):
```

Entering a passphrase does have its benefits, but the downside, of course, is to having a passphrase, is then having to type it in each time you use the key pair. So press `Enter`.

The entire key generation process looks like this:

```bash
$ ssh-keygen -t rsa -b 4096
```

```bash
Generating public/private rsa key pair.
Enter file in which to save the key (/home/tiger/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/tiger/.ssh/id_rsa.
Your public key has been saved in /home/tiger/.ssh/id_rsa.pub.
The key fingerprint is:
4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 tiger@a
The key's randomart image is:
+--[ RSA 4096]----+
|          .oo.   |
|         .  o.E  |
|        + .  o   |
|     . = = .     |
|      = S = .    |
|     o + = +     |
|      . o + o .  |
|           . o   |
|                 |
+-----------------+
```

## Copying the Public Key

Once the key pair is generated, it’s time to place the public key on Rostam.

You can copy the public key into the new machine’s `authorized_keys` file with the `ssh-copy-id` command. Make sure to replace the example username.

```bash
ssh-copy-id myuser@rostam.cct.lsu.edu
```

If you installed the ssh key on different location in previus step you can point to it by `-i [path-to-key]` option.

> Note:  If you are a Mac user, ssh-copy-id will not be installed on your machine. You can, however, install it using Homebrew:
> ```bash
> $ brew install ssh-copy-id
> ```

Alternatively, you can paste in the keys using SSH:

```bash
$ cat ~/.ssh/id_rsa.pub | ssh myuser@rostam.cct.lsu.edu "cat >> ~/.ssh/authorized_keys"
```

Now you should be able to login to Rostam without typing password. Give it a try by:

```bash
ssh myuser@rostam.cct.lsu.edu
```

# Using the SSH Config File

Now that we took care of the password let's automate the process as much as we can. The SSH client-side configuration file is named `config`, and it is stored in `.ssh` directory under user’s home directory. (same for Windows, Mac and Linux)

The ~/.ssh directory is automatically created when the user runs the ssh command for the first time and this is the place we stored our keys in previus step. 

By default the SSH configuration file may not exist so you may need to create it.

```bash
$ touch ~/.ssh/config
```

The SSH client is peculiarly picky about its config file permissions. This file must be readable and writable only by the user, and not accessible by others:

```bash
$ chmod 600 ~/.ssh/config
```

Now open the config file with editer of choice
```bash
$ vim ~/.ssh/config
```

and add following config:

```ssh-config
Host rostam
	Hostname rostam.cct.lsu.edu
  Port 22		# Default Port
	User myuser
	IdentityFile ~/.ssh/id_rsa  # Default Path
```

Now go ahead and give it a try by using the `Host` option:

```bash
$ ssh rostam
```

And you should be in, no question asked.

# Connecting to a Compute Node Directly

Rostam uses its own internal network. The compute nodes are only connected to this internal network and can access the internet in one-direction through a firewall. The one-direction connection means any communication must start from inside, no one can initiate a communication from outside.

You can access the compute nodes through the login node, which itself is behind a firewall but lets in the authorized users. Now let's use this notion and setup ssh to access through the login node.

Let's assume in this example you want to directly access `geev` which is cuda node with two v100 cards installed.

Open the `.ssh/config` file and create an entry for geev the same way you already created for rostam. Note that all compute nodes share login information and filesystem, so your key is already in all nodes when copied it to login node. Then instruct the SSH client to do a proxy jump through login node by adding `ProxyJump rostam` to the entry for geev. The end result should look like this:

```ssh-config
Host rostam
	Hostname rostam.cct.lsu.edu
  Port 22		# Default Port
	User myuser
	IdentityFile ~/.ssh/id_rsa  # Default Path
  
Host geev
	Hostname geev.rostam.cct.lsu.edu
  Port 22		
	User myuser
	IdentityFile ~/.ssh/id_rsa 
  ProxyJump rostam
```

Now let's give our new configuration a try.

```bash
$ ssh geev
Access denied by pam_slurm_adopt: you have no active jobs on this node
```

This is Slurm that prevents unauthorized, you can not login to a node unless you have an active job there and the solution is easy let's create job:

```bash
scalloc -p v100 -N 1 -t 1:00:00
```

This command allocates bahram for one hour to the user.