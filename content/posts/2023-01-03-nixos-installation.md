---
title: "Installing NixOS as my main OS"
date: 2023-01-03
tags:
  - nix
  - nixos
  - linux
summary: My notes when installing NixOS
---

{{< alert >}}
What follows are my hastily written notes and experiences when installing NixOS as my main operating system.
{{< / alert >}}

Today I decided to install NixOS onto a secondary hard drive installed into my desktop, currently running Arch Linux.

# Hardware

I installed a second M.2 SSD into my second M.2 slot. My plan is to install NixOS onto the new hard drive, configure it as a boot drive and leave my Arch install alone. I am led to believe that NixOS will observe already present boot partitions and add them to GRUB automatically. Let's see!

I've already burnt the 22.11 NixOS KDE live CD to USB drive, here goes...

## Work out what my BIOS/UEFI knows my existing drive as

I stupldly bought the same brand/model of SSD so this might be fun. Luckily it's in the M2_1 slot, and the BIOS labels the slots, so 🎉

## Install the HDD

Initially there was a potential issue - I couldn't see the M2 port, however it looks like ASUS has installed some covers for parts of the motherboard. Once I removed this, I found the port ok, and installation was straightforward.

The new drive shows up in the `lsblk` output under `nvme0n1`

```
$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
nvme1n1     259:0    0 931.5G  0 disk
├─nvme1n1p1 259:2    0   512M  0 part /efi
├─nvme1n1p2 259:3    0    16G  0 part [SWAP]
└─nvme1n1p3 259:4    0   915G  0 part /
nvme0n1     259:1    0 931.5G  0 disk
```

and is present in `gparted`.

{{< alert color="info" >}}
At least this means I can use the new HDD even if this NixOS thing doesn't pan out...
{{< / alert >}}

# OS

## Booting into the live USB

{{< alert color="info" >}}
For some reason my Arch install takes AGES to shut down... 🤷‍♂
{{< / alert >}}

This worked out fine, I used the BIOS to manually boot into the USB drive, rather than change boot order.


## Installation

The installation worked fine

## Booting into the new system

The new system boots but does not load any GUI

I installed the nvidia drivers using [this wiki article](https://nixos.wiki/wiki/Nvidia#Installing_NVIDIA_official_drivers_on_NixOS)

I got a graphical log in console (SDDM) and KDE boots while observing the correct monitor layout 🎉

Wow this is amazing! I have a new linux system set up. Now for the ultimate test: updating my configuration and using the new system

## Checking the previous install still works

👍

## Setting up the system as I like it

{{< alert color="info" >}}
I LOVE NIXOS - the ability to roll back the configuration to a previously working one is a life-saver!
{{< / alert >}}


Installing things is relatively painless. I had to make some tweaks to my configuration, namely installing the `nvidia` drivers

```nix
# in hardware-configuration.nix
hardware.opengl.enable = true;
hardware.opengl.driSupport32Bit = true;  # for steam

# in configuration.nix
services.xserver.videoDrivers = [ "nvidia" ];
```

I had a minor problem with `rust-fitsio` not finding `/bin/cp` which I worked around by patching the `cfitsio` source code, but equally could probably have used a `fhsUserEnv` if I knew more about them.
