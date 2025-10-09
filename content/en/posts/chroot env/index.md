---
title: Chroot - A lifesaver for linux users
date: 2025-07-01T01:43:13+05:30
author: Arnav Gupta
description: A lifesaver for linux user
---
---

![Chroot Linux](https://www.maketecheasier.com/assets/uploads/2022/07/chroot-linux.jpg)

A **Chroot environment** is a way to run programs and commands in modified **root** directory, the similar way we use **root** in our terminal.

By utilizing **chroot**, you can separate the execution environment of a program, establishing a controlled area where only designated files and directories can be accessed. This is especially beneficial for <u>system recovery</u>, security assessments, and setting up isolated environments for particular applications.

## Different Use Cases of Chroot

1. **Isolated CI/CD Builds:** Use chroot in CI/CD pipelines to create isolated build environments, preventing dependency conflicts.

2. **Development and Testing:** Ensure software runs on all devices by testing in a clean chroot environment, mimicking end-user systems.

3. **Risk Reduction:** Developers use chroot to create safe environments, reducing the risk of data loss from system file interactions.

4. **Software Version Management:** Install different software versions in separate chroot environments to avoid system disruptions.

5. **System Recovery:** Repair broken systems by booting a live Linux environment and using chroot to execute repair commands.

6. **Secure FTP Server:** Run an FTP server within a chroot to control file access and protect the host file system.

## Note:

**A program that is run in such a modified environment cannot access files and commands outside that environmental directory tree. This modified 
environment is called a *<mark>chroot jail</mark>*.**

![https://linuxtldr.com/wp-content/uploads/2022/12/chroot-jail-1024x675.webp](https://linuxtldr.com/wp-content/uploads/2022/12/chroot-jail-1024x675.webp)

---

## How to use *Chroot* to fix broken system - *System recovery* ?

If you have faced System failures such as system **not booting**, or you may have unknowingly **removed** some important packages or some dependencies that may have caused issues in your distro.. then Chroot will nothing less than a lifesaver 🛟 for your linux distro. 

There are other ways to fix like, going to terminal while booting, where you can use ther terminal like the whole OS is woking on terminal. But that didn't worked for me 🥲

So I removed some important endevour os system packages, in order to clean my **Arch** 🧹.

Later I noticed that my some system functionalites had stopped working and cant update the **pacman** **mirrorlist** in order to fix and install the missing packages,  that i removed.. And when I tried to reboot the system, the system **wont boot**.

So inorder to fix this, I used chroot. 😮‍💨

- *For more help for how to use chroot reffer [chroot - ArchWiki](https://wiki.archlinux.org/title/Chroot)*


### Create a live usb of your installed distro

Creating a live usb is the main part to use chroot because you are going to use ***chroot*** on the terminal of the live usb terminal

### Mounting the partition in which your distro is installed

Mount your installed distro installed partition using the following command -

```bash
mount /dev/sdXY /mnt
```

The *chroot* target should be a directory which contains a file system hierarchy.

In the [installation guide](https://wiki.archlinux.org/title/Installation_guide "Installation guide"), this directory would be `/mnt`. For an existing installation, you need to mount existing partitions into `/mnt` yourself:

Run `lsblk` and note the partition layout of your installation. It will be usually something like `/dev/sdXY` or if you have an NVMe drive `/dev/nvme0nXpY`.

### Enter a chroot

<u></u>Run *arch-chroot* with the new root directory as first argument - 

```bash
arch-chroot */path/to/new/root
```

### Using chroot

If you run *chroot* directly, below steps are needed before actual *chroot*.

First, mount the temporary API filesystems -

```bash
cd */path/to/new/root*
mount -t proc /proc proc/
mount -t sysfs /sys sys/
mount --rbind /dev dev/
```

##### The enter the chroot env:

```bash
chroot /mnt/customroot 
```

### Install packages via Pacman

Then you can use the terminal and operate it for the installed distro

###### <mark>Note</mark> : *You can only install <u>system packages</u> from chroot, you cannot intall user application such as firefox, vscode etc as according to pacman i caused some dependencies issues* 🤷🏻‍♂️.

## Exit Chroot environment

* If you are still inside the chroot environment, make sure to exit it first

```bash
exit
```

* **Unmount the Filesystems:**

You should unmount the filesystems in the reverse order of how they were mounted. Typically, you would do it as follows - 

```bash
umount /path/to/new/root/dev
umount /path/to/new/root/sys
umount /path/to/new/root/proc
```

Replace `/path/to/new/root` with the actual path to your chroot environment.

* **Unmount the Root Filesystem:**

Finally, unmount the root filesystem of the chroot environment:

```bash
umount /path/to/new/root
```

## How to update the Pacman mirrorlist from live usb.

1. In order to fix this, I **used EndevourOS reflector** installed in the **live USB**, or simple Arch reflector can also work .

2. Updated the mirrorlist , selecting **all the countries** , with **100** mirros and filtering them according to there **transfer speed.**

3. Then I copied the mirrorlists from the `mirrorlist` file , located in `/etc/pacman.d/mirrorlist` directory and pasted in the same directory but in my installed system directory.

4. Then you can use **chroot** to Update the pachages that are installed via pacman. 

#### References :

* [ArchWiki](https://wiki.archlinux.org/title/Chroot)

* [Make Tech Easier](https://www.maketecheasier.com/use-chroot-linux/)

* [DEV Community](https://dev.to/robogeek95/creating-a-chrooted-environment-1gkd)

### *Hope this  blog will help* 🫡.
