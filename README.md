# Introduction to Multipass on Ubuntu Linux

***Tom Dean - 2/2/23, updated 11/4/26***

###### tags: `Ubuntu`, `Linux`, `Multipass`, `cloud-init`, `virtualization`, `Intel`, `snap`

## Introduction

Wouldn't it be nice if you could have your own personal mini-cloud running in the confines of your Ubuntu Linux environment?  Now you can, thanks to Multipass!

**Per Canonical:**

"Multipass is a tool to generate cloud-style virtual machines (VMs) quickly on Linux, macOS and Windows."

**From the Multipass GitHub repository:**

"Multipass is a lightweight VM manager for Linux, Windows and macOS. It's designed for developers who want to spin up a fresh Linux environment with a single command. It uses QEMU on Linux, HCS (Host Compute System) on Windows and QEMU or the Apple Virtualization Framework on macOS to run virtual machines with minimal overhead. Multipass will fetch images for you and keep them up to date.

Since it supports metadata for cloud-init, you can simulate a small cloud deployment on your laptop or workstation."

Multipass makes running Ubuntu virtual machines fast and easy.

***Let's see how we set it up!***

## References

[Multipass](https://canonical.com/multipass)

[Multipass Documentation](https://canonical.com/multipass/docs/stable/)

[Install Multipass](https://canonical.com/multipass/install)

[Using cloud-init with Multipass](https://ubuntu.com/blog/using-cloud-init-with-multipass)

[Multipass Tutorial](https://canonical-multipass1.readthedocs-hosted.com/stable/tutorial/)

[Multipass: GitHub Repository](https://github.com/canonical/multipass)

## Install Multipass Using `snap`

Installation is straightforward, using `snap`:
```bash
sudo snap install multipass
```

```bash
multipass 1.11.0 from Canonical✓ installed
```

### *Uninstall Multipass: OPTIONAL*

If you'd like to *uninstall* Multipass, you can use:
```bash
sudo snap remove multipass
```

```bash
2023-02-03T00:42:15Z INFO Waiting for "snap.multipass.multipassd.service" to stop.
multipass removed
```

### Quick Test: Multipass

Let's validate our Multipass installation by checking the version:
```bash
multipass version
```

We should get a result similar to:
```bash
multipass   1.17.0
multipassd  1.17.0
```

*Remember, versions and architectures can vary based on when you install and what you install it on!*

***Now that Multipass is installed, let's try it out!***

## Working With Multipass

### Deploy Your First Instance

Before we get started, let's take a look at the online help available with Multipass:
```bash
multipass help
```

```bash
Usage: multipass [options] <command>
Create, control and connect to cloud instances.

This is a command line utility for multipass, a
service that manages cloud instances.

Options:
  -h, --help     Displays help on commandline options
  -v, --verbose  Increase logging verbosity. Repeat the 'v' in the short option
                 for more detail. Maximum verbosity is obtained with 4 (or more)
                 v's, i.e. -vvvv.

Available commands:
  alias          Create an alias
  aliases        List available aliases
  authenticate   Authenticate client
  clone          Clone an instance
  delete         Delete instances and snapshots
  disable-zones  Make zones unavailable
  enable-zones   Make zones available
  exec           Run a command on an instance
  find           Display available images to create instances from
  get            Get a configuration setting
  help           Display help about a command
  info           Display information about instances or snapshots
  launch         Create and start a cloud instance
  list           List all available instances or snapshots
  mount          Mount a local directory in the instance
  networks       List available network interfaces
  prefer         Switch the current alias context
  purge          Purge all deleted instances permanently
  recover        Recover deleted instances
  restart        Restart instances
  restore        Restore an instance from a snapshot
  set            Set a configuration setting
  shell          Open a shell on an instance
  snapshot       Take a snapshot of an instance
  start          Start instances
  stop           Stop running instances
  suspend        Suspend running instances
  transfer       Transfer files between the host and instances
  umount         Unmount a directory from an instance
  unalias        Remove aliases
  version        Show version details
  wait-ready     Wait for the Multipass daemon to be ready
  zones          List all availability zones
```

Let's try deploying an instance.  We can use the `multipass find` command to retrieve a list of available images:
```bash
multipass find
```

This should give us output similar to the following:
```bash
Image             Aliases                     Version          Description
22.04             jammy                       20261020         Ubuntu 22.04 LTS
24.04             noble                       20261017         Ubuntu 24.04 LTS
26.04             resolute,lts,ubuntu         20261016         Ubuntu 26.04 LTS
26.10             stonking                    20261025         Ubuntu 26.10
core:core16                                   current          Ubuntu Core 16
core:core18                                   current          Ubuntu Core 18
core:core20                                   current          Ubuntu Core 20
core:core22                                   current          Ubuntu Core 22
core:core24                                   current          Ubuntu Core 24
core:core26                                   current          Ubuntu Core 26
debian            trixie                      20261001         Debian 13
fedora                                        20260422         Fedora 44
```

We'll launch the `lts` image, which, along with `resolute`, is aliased to the `26.04` image.  We can use any of those to launch the image.  The `lts` alias moves to each new LTS release, so you may see a newer version than the one shown here.

Using the `multipass launch` command:
```bash
multipass launch lts
```

Once the instance is up, we'll get a `Launched:` message, along with our random instance name and the availability zone it landed in:
```bash
Launched: touched-dhole in zone1
```

We can see a list of instances using the `multipass list` command:
```bash
multipass list
```

We should see something like this:
```bash
Name                    State             IPv4             Image               Zone
touched-dhole           Running           10.97.0.221      Ubuntu 26.04 LTS    zone1
```

If we want to get detailed information about our instance, we can use the `multipass info` command:
```bash
multipass info touched-dhole
```

We should see output similar to this:
```bash
Name:           touched-dhole
State:          Running
Zone:           zone1
Snapshots:      0
IPv4:           10.97.0.221
Release:        Ubuntu 26.04.1 LTS
Image hash:     5645d0318ad2 (Ubuntu 26.04 LTS)
CPU(s):         1
Load:           1.53 0.67 0.25
Disk usage:     2.2GiB out of 4.8GiB
Memory usage:   179.7MiB out of 950.3MiB
Mounts:         --
```

This gives us a quick summary of our `touched-dhole` instance's vital statistics.

***Deploying an instance was fast and easy!  Now, how do we interact with this instance?***

### Interacting With Our Multipass Instance

Now that we have an instance up and running, let's take a look at a couple of ways to interact with it.

One way to interact with our instance is to open a shell in our instance using the `multipass shell` command:
```bash
multipass shell touched-dhole
```

Let's take a look at the contents of the `/etc/os-release` file to get some information on our instance:
```bash
ubuntu@touched-dhole:~$ cat /etc/os-release
PRETTY_NAME="Ubuntu 26.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04.1 LTS (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=resolute
LOGO=ubuntu-logo
```

So, we can interact with our instance using a shell, just like any other Linux machine.  Type `exit` when you're done, to get back to your host.

We might want to execute a command without opening a shell.  We can do this with the `multipass exec` command:
```bash
multipass exec touched-dhole -- cat /etc/os-release
```

We see the same information as when we executed the command in a shell, without the interactive shell session:
```bash
PRETTY_NAME="Ubuntu 26.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="26.04"
VERSION="26.04.1 LTS (Resolute Raccoon)"
VERSION_CODENAME=resolute
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=resolute
LOGO=ubuntu-logo
```

### Cleaning Up

The beauty of having our own little private cloud is that we can easily get rid of resources that we're finished with.

Let's take a look at our instances:
```bash
multipass list
```
We see our `touched-dhole` instance:
```bash
Name                    State             IPv4             Image               Zone
touched-dhole           Running           10.97.0.221      Ubuntu 26.04 LTS    zone1
```

First, let's stop our instance:
```bash
multipass stop touched-dhole
```

Now that our `touched-dhole` instance is stopped, let's take a look:
```bash
multipass list
```

We can see that our instance is stopped:
```bash
Name                    State             IPv4             Image               Zone
touched-dhole           Stopped           --               Ubuntu 26.04 LTS    zone1
```

We'll delete our `touched-dhole` instance:
```bash
multipass delete touched-dhole
```

Checking our work:
```bash
multipass list
```

We can confirm our `touched-dhole` instance has been deleted:
```bash
Name                    State             IPv4             Image               Zone
touched-dhole           Deleted           --               Ubuntu 26.04 LTS    zone1
```

If you want to completely remove the `touched-dhole` instance, you can use the `multipass purge` command:
```bash
multipass purge
```

Checking our work:
```bash
multipass list
```

We see no instances:
```bash
No instances found.
```

***That was easy!  Good stuff!***

### Digging Deeper

If you'd like to dig a little deeper into running instances using Multipass, here are some good resources:

[Using cloud-init with Multipass](https://ubuntu.com/blog/using-cloud-init-with-multipass)

[Multipass Tutorial](https://canonical-multipass1.readthedocs-hosted.com/stable/tutorial/)

## Summary

By using Multipass, we can leverage the power of Ubuntu Linux and `cloud-init` to manage our own little private cloud, in the comfort of our Ubuntu Linux environment.

Enjoy!

*Tom Dean*

## License

This guide is licensed under [CC BY 4.0](LICENSE).
