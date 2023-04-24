# Introduction to Multipass on Ubuntu Linux

***Tom Dean - 2/2/23***

###### tags: `Ubuntu`, `Linux`, `Multipass`, `cloud-init`, `virtualization`, `Intel`, `apt-get`, `Kubernetes`, `Debian`

## Introduction

Wouldn't it be nice if you could have your own personal mini-cloud running in the confines of your Ubuntu Linux environment?  Now you can, thanks to Multipass!

**Per Canonical:**

"Multipass is a tool to generate cloud-style Ubuntu VMs quickly on Linux, macOS, and Windows. It gives you a simple but powerful CLI that allows you to quickly access an Ubuntu command line or create your own local mini-cloud."

**From the Multipass GitHub repository:**

"Multipass is a lightweight VM manager for Linux, Windows and macOS. It's designed for developers who want a fresh Ubuntu environment with a single command. It uses KVM on Linux, Hyper-V on Windows and HyperKit on macOS to run the VM with minimal overhead. It can also use VirtualBox on Windows and macOS. Multipass will fetch images for you and keep them up to date.

Since it supports metadata for cloud-init, you can simulate a small cloud deployment on your laptop or workstation."

Multipass makes running Ubuntu virtual machines fast and easy.

***Let's see how we set it up!***

## References

[Multipass](https://multipass.run)
[Multipass Documentation](https://multipass.run/docs)
[Install Multipass](https://multipass.run/install)
[Using cloud-init with Multipass](https://ubuntu.com/blog/using-cloud-init-with-multipass)
[Multipass Tutorial: Linux](https://multipass.run/docs/linux-tutorial)
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
multipass   1.11.0
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
Create, control and connect to Ubuntu instances.

This is a command line utility for multipass, a
service that manages Ubuntu instances.

Options:
  -h, --help     Displays help on commandline options
  -v, --verbose  Increase logging verbosity. Repeat the 'v' in the short option
                 for more detail. Maximum verbosity is obtained with 4 (or more)
                 v's, i.e. -vvvv.

Available commands:
  alias         Create an alias
  aliases       List available aliases
  authenticate  Authenticate client
  delete        Delete instances
  exec          Run a command on an instance
  find          Display available images to create instances from
  get           Get a configuration setting
  help          Display help about a command
  info          Display information about instances
  launch        Create and start an Ubuntu instance
  list          List all available instances
  mount         Mount a local directory in the instance
  networks      List available network interfaces
  purge         Purge all deleted instances permanently
  recover       Recover deleted instances
  restart       Restart instances
  set           Set a configuration setting
  shell         Open a shell on a running instance
  start         Start instances
  stop          Stop running instances
  suspend       Suspend running instances
  transfer      Transfer files between the host and instances
  umount        Unmount a directory from an instance
  unalias       Remove aliases
  version       Show version details
```

We'll launch the `lts` image, which, along with `jammy`, is aliased to the `22.04` image.  We can use any of those to launch the image.

Using the `multipass launch` command:
```bash
multipass launch lts
```

Once the instance is up, we'll get a `Launched:` message, along with our random instance name:
```bash
PUT OUTPUT HERE
```

We can see a list of instances using the `multipass list` command:
```bash
multipass list
```

We should see something like this:
```bash
PUT OUTPUT HERE
```

If we want to get detailed information about our instance, we can use the `multipass info` command:
```bash
multipass info unanimous-harrier
```

We should see output similar to this:
```bash
PUT OUTPUT HERE
```

This gives us a quick summary of our `unanimous-harrier` instance's vital statistics.

***Deploying an instance was fast and easy!  Now, how do we interact with this instance?***

### Interacting With Our Multipass Instance

Now that we have an instance up and running, let's take a look at a couple of ways to interact with it.

One way to interact with our instance is to open a shell in our instance using the `multipass shell` command:
```bash
multipass shell unanimous-harrier
```

Let's take a look at the contents of the `/etc/os-release` file to get some information on our instance:
```bash
PUT OUTPUT HERE
```

So, we can interact with our instance using a shell, just like any other Linux machine.

We might want to execute a command without opening a shell.  We can do this with the `multipass exec` command:
```bash
multipass exec unanimous-harrier -- cat /etc/os-release
```

We see the same information as when we executed the command in a shell, without the interactive shells session:
```bash
PUT OUTPUT HERE
```

### Cleaning Up

The beauty of having our own little private cloud is that we can easily get rid of resources that we're finished with.

Let's take a look at our instances:
```bash
multipass list
```
We see our `unanimous-harrier` instance:
```bash
PUT OUTPUT HERE
```

First, let's stop our instance:
```bash
multipass stop unanimous-harrier
```

Now that our `unanimous-harrier` instance is stopped, let's take a look:
```bash
multipass list
```

We can see that our instance is stopped:
```bash
PUT OUTPUT HERE
```

We'll delete our `unanimous-harrier` instance:
```bash
multipass delete unanimous-harrier
```

Checking our work:
```bash
multipass list
```

We can confirm our `unanimous-harrier` instance has been deleted:
```bash
PUT OUTPUT HERE
```

If you want to completely remove the `unanimous-harrier` instance, you can use the `multipass purge` command:
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

### Using `minikube` in Multipass



### Digging Deeper

If you'd like to dig a little deeper into running instances using Multipass, here are some good resources:

[Using cloud-init with Multipass](https://ubuntu.com/blog/using-cloud-init-with-multipass)
[Multipass Tutorial: Linux](https://multipass.run/docs/linux-tutorial)

## Summary

By using Multipass, we can leverage the power of Ubuntu Linux and `cloud-init` to manage our own little private cloud, in the comfort of our Ubuntu Linux environment.

Enjoy!

*Tom Dean*
