---
title: Connetcing to Rostam
description: How to connect via ssh to Rostam
published: true
date: 2020-03-03T21:33:12.590Z
tags: 
---

# Accessing the System

The `ssh` command (SSH protocol) is the standard way to connect to Rostam. SSH also includes support for the file transfer utilities `scp` and `sftp`. [Wikipedia](https://en.wikipedia.org/wiki/Secure_Shell) is a good source of information on SSH. SSH is available within Linux and from the terminal app in the Mac OS. If you are using Windows, you will need an SSH client that supports the SSH-2 protocol: e.g. [Bitvise](http://www.bitvise.com/), [OpenSSH](http://www.openssh.com/), [PuTTY](http://www.putty.org/), or [SecureCRT](https://www.vandyke.com/products/securecrt/). Initiate a session using the ssh command or the equivalent; from the Linux command line the launch command looks like this:

```bash
localhost$ ssh myuser@rostam.cct.lsu.edu
```

To connect with X11 support on Rostam:
```bash
localhost$ ssh -X myuser@rostam.cct.lsu.edu
```

# Two-step Authentication
Access to all LSU systems now requires Two-Factor Authentication (2FA). You can set up your two-factor authentication after you logged in for the first time. Please follow the instructions [Two-step Authentication Setup](https://wiki.rostam.cct.lsu.edu/en/ssh/otp)