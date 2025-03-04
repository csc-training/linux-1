---
title:	Basics of Linux - Obtain and access
author:	CSC Training / Joona Tolonen
date:	  2025-01
lang:	  en
---

# Obtaining a Linux machine
Because Linux is a computer operating system, you'll need a computer (real or virtual) to run it. This chapter discusses the options for getting one.

> [!NOTE]
> If you are a student at a Finnish higher education institution, you can use CSC's environments free of charge to e.g. try out Linux.

## Exercises
1. Dedicated hardware gives you the most options and the best performance.
  - Check out the article [How to dual-boot Linux and Windows](https://opensource.com/article/18/5/dual-boot-linux).
  - Another option for dedicated hardware could be an affordable [Raspberry Pi](https://en.wikipedia.org/wiki/Raspberry_Pi) or equivalent.
2. A locally virtualised environment gives you flexibility with less risk of disrupting your primary operating system.
  - Check out the article [Kernel-based Virtual Machine](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine) for Linux kernels.
  - For Microsoft-based environments, [Hyper-V](https://en.wikipedia.org/wiki/Hyper-V) is available.
  - There are also cross-platform solutions such as [Virtualbox](https://en.wikipedia.org/wiki/VirtualBox) and others.
3. Cloud environments provide remote (virtualised) machines, typically running a customised Linux operating system.
  - The price of commercial environments such as AWS, Azure, Google, Hetzner, Oracle, etc. is a combination of many factors. Compare them and choose the best one for your needs.
  - CSC's [cloud](https://docs.csc.fi/cloud/) and [computing](https://docs.csc.fi/computing/) services are free[^1] for students and staff at Finnish universities. [cPouta](https://docs.csc.fi/pouta) is an IaaS service. Check out the [range of different flavours](https://docs.csc.fi/cloud/pouta/vm-flavors-and-billing/).

> [!NOTE]
> The exercises in this course can technically be done in any Linux environment, but in this course we will use CSC's Linux environments.

# Obtaining a Linux distribution
> Most Linux distributions are free, but maintained by volunteers. If possible, participate in the community work or make donations.

> [!TIP]
> If you are using CSC's cloud environment, it is optional to go through exercises that handles installing Linux.

## Exercises
1. Dedicated hardware or locally virtualised environments give you complete freedom to choose the distribution you want.
  - If you have not already done so, visit [distrowatch.com](https://distrowatch.com) and the [List of Linux distributions](https://en.wikipedia.org/wiki/List_of_Linux_distributions) wiki page. Choose a distribution that best suits your needs.
  - If you are installing Linux on physical hardware, you may need a bootable USB stick. Tools such as [Rufus](https://rufus.ie/en/) and [Etcher](https://etcher.balena.io/) are available to create one.
2. Hosted platforms typically only offer pre-defined operating systems that are best suited to the platform being run.
  - If you are interested or have chosen a commercial cloud provider, have a look at their range of different flavours and operating systems.
  - Read the basic information about [images](https://docs.csc.fi/cloud/pouta/images/) - especially the default distributions available part - in the Pouta environment.
3. If you already have an existing Linux machine, check the details of the operating system using the command `cat /etc/os-release` from the command-line.
4. :cactus: The Pouta environment supports custom images that can be run in the environment. Read the procedure [Creating, converting, uploading and sharing virtual machine images](https://docs.csc.fi/cloud/pouta/adding-images/). :cactus:

# Accessing a Linux
> Operating systems can be accessed remotely or locally. Both remote and local Linux machines can be accessed through both graphical and command-line interfaces. The command line interface is superior for remote Linux machines.

## Exercises
1. If you have a locally installed Linux operating system, try `Ctrl + Alt + F1` to `F7`. Many distributions support virtual consoles that can be accessed using this key combination. The graphical user interface is often located at `F7`.
2. If you have a Linux or MacOS operating system on your computer, you can connect to a remote Linux machine using the terminal and an `ssh` command.
3. If you are using Windows and want to connect to remote Linux and its command line, you can use third-party software such as [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) or the built-in PowerShell command prompt.
4. :cactus: If you have a remote Linux machine and want to use a graphical user interface, you'll need a [VNC server](https://en.wikipedia.org/wiki/VNC) or similar. Also remember that the Linux operating system may come without a GUI installed, so you will need to install that as well. :cactus:

# Graphical user interface

## Exercises
1. 
- X11 and Wayland
- GNOME, KDE, XFCE, ...
-  

# Command-line interface

## Exercises
