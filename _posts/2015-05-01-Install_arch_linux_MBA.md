---
layout: post
title: Install Arch Linux on MacBook Air 6,1
date: 2015-05-01
categories:
- Linux
- Arch Linux
- Informatique
- In English
author: Joris Muller
---

One of my colleague suggested me to try Arch Linux several months ago. I'm a Mac OS user since more than two decades but I'm using Linux on my servers and some devices like the Rasberry Pi or some old computers. Currently, most of the softwares I'm using are open-source: Firefox, Libreoffice, Latex, Vim, Inkscape, Gimp... Furthermore, I start to be tired with the Apple's softwares. For example, I was fan of Aperture. Apple just decided to stop this product.

Well, in this post, I will try to describe the installation of Arch Linux on my MacBook Air 6.1 (i7) in dual boot with an existing OS X.

# Bootloader strategy

The hardest part of the installation on mac is, I believe, the bootloader. There is to my knowledge 4 alternatives :

1. A [Standalone GRUB 2](https://wiki.archlinux.org/index.php/GRUB#GRUB_standalone) on the `/BOOT` partition. I will follow this way.
2. rEFInd
3. rEFIt
4. Another one I forgot

# Documentation

To perform the installation, I used the following pages :

- [Install Arch on iMac](https://wiki.archlinux.org/index.php/IMac_Fusion#Proceed_installing_Archlinux)
- [Standalone GRUB 2](https://wiki.archlinux.org/index.php/GRUB#GRUB_standalone)
- [Arch installation guide](https://wiki.archlinux.org/index.php/Installation_guide)

Additionnal pages :

- [Arch's Forum topic about MacBook Air](https://bbs.archlinux.org/viewtopic.php?id=184464)
- [arch wiki page for macbook](https://wiki.archlinux.org/index.php/MacBook) is not up-to-date. 

# Short installation guide

- __Backup__ your data! Make an Image Disk and and Backup with Time Machine.
- [Partition](https://wiki.archlinux.org/index.php/MacBook#OS_X_with_Arch_Linux)
- [Download](https://www.archlinux.org/download/) Arch 
- [Create an USB installation drive](https://wiki.archlinux.org/index.php/USB_flash_installation_media#In_Mac_OS_X). On OS X, find the USB device with `diskutil list` and write the USB device with `dd if=Downloads/archlinux-2015.05.01-dual.iso of=/dev/rdisk2 bs=1m` (assuming your USB device is `/dev/sda3`)
- Reboot on USB drive holding down the <kbd>Alt</kbd> Key.


# Partition scheme

```
/dev/disk0
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *251.0 GB   disk0
   1:                        EFI EFI                     209.7 MB   disk0s1
   2:          Apple_CoreStorage                         209.2 GB   disk0s2
   3:                 Apple_Boot Recovery HD             650.1 MB   disk0s3
   4:       Microsoft Basic Data                         39.2 GB    disk0s4
   5:                 Linux Swap                         1.7 GB     disk0s5
/dev/disk1
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:                  Apple_HFS Macintosh HD           *208.9 GB   disk1
                                 Logical Volume on disk0s2
                                 C84C5CBA-291A-42CB-B6A6-CF8AE01B9DF5
                                 Unlocked Encrypted
```

# Post-install

There is lot of bugs after initial installation. Let's fix them

## Wifi

## Keyboard layout

The Keyboard layout is a mess. The fr-mac is not good at all. @#<> are in the wrong place and I can't find the brackets. 

## Brighnest level on/off after suspend

As described in [this post](https://github.com/jomuller/jomuller.github.io.git), the backlight can't be set well after suspend.
 
There is a solution with a patch.

First, update the whole system

```
sudo pacman -Syu
```

Maybe the wifi driver will be broken, then reinstall it if the kernel was reinstalled.

Upgrade

```sudo pacman -Syu```

Then restart if linux kernel was upgraded.

Install linux headers

```sudo pacman -S linux-headers```


Install the mba6x_bl-dkms patch from AUR

```
wget https://aur.archlinux.org/packages/mb/mba6x_bl-dkms/mba6x_bl-dkms.tar.gz
tar -xvf mba6x_bl-dkms.tar.gz
cd mba6x_bl-dkms
makepkg -i
```

## Touchpad
