---
layout: post
title: "노트북에 Arch Linux 설치하기"
comments: true
author: feynubrick
date: 2019-01-16
tags: [Linux, How-to]
---

## Arch linux?


the following installation process are based on archwiki

## Pre-installation

### verify boot mode
check the system has been booted on UEFI or not

```
# ls /sys/firmware/efi/efivars
```

there are bunch of things!
that means it's in UEFI boot mode.

### connect to internet
I will use wifi

#### check the driver status
check if the driver for my card loaded

```
# lspci -k | grep -iA3 "network"
```

the `k` option means "show kernel drivers handling each device and also kernel modules capable of handling it", according to man page.
the output is:
```
02:00.0 Network controller: Intel Corporation Wireless 8265 / 8275 (rev 78)
        Subsystem: Intel Corporation Wireless 8265 / 8275
        Kernel driver in use: iwlwifi
        Kernel modules: iwlwifi, wl
```

check if a wireless interface was created by do the following:

```
# ip link
```

there is `wlp2s0` which looks like wifi device
then bring the interface up with:

```
# ip link set wlp2s0 up
```

no output => check the messages for the driver module named `iwlwifi`
```
# dmesg | grep iwlwifi
```

the message is like:
```
[    12.870378] iwlwifi 0000:02:00.0: enabling device (0000 -> 0002)
[    13.171346] iwlwifi 0000:02:00.0: loaded firmware version 36.e91976c0.0 op_mode iwlmvm
[    13.252395] iwlwifi 0000:02:00.0: Detected Intel(R) Dual Band Wireless AC 8265, REV=0x230
```
the kernel module is successfully loaded and the interface is up!

#### connect to wifi network
using `netctl` instead

```
# cp /etc/netctl/examples/wireless-wpa /etc/netctl/wifi_WHATEVER_YOU_WANT
# vim wifi_WHATEVER_YOU_WANT
```

and edit the respective values as follows:

```
...
Interface=wlp2s0
...
ESSID='WIFI NAME'
...
Key='PASSWORD'
```

start `netctl`

```
# netctl start wifi_WHATEVER_YOU_WANT
```

if there is error, refer the contents of the error by following command:

```
# journalctl -xe
```

if the error message is "The interface of network profile 'wifi_WHATEVER_YOU_WANT' is already up",
then do the following and try again.

```
# ip link set wlp2s0 down
```

if there is no error, check th` connection by following:

```
# ping google.com
```

## update the system clock

```
# timedatectl set-ntp true
```

## partitioning

check the current status
# fdisk -l
to edit the partition,
# cfdisk /dev/nvme0n1
delete all partitions and make new partitions
name
mount point
partition name
size
file system
format
boot
/boot
nvme0n1p1
550M
EFI system
FAT32
swap
none
nvme0n1p2
2G
Linux swap
swap
root
/
nvme0n1p3
32G
Linux root (x86-64)
ext4
home
/home
nvme0n1p4
Linux home
ext4
check the partition table again
# fdisk -l
## format the partitions
# mkfs.fat -F32 /dev/nvme0n1p1
# mkswap /dev/nvme0n1p2
# swapon /dev/nvme0n1p2
# mkfs.ext4 /dev/nvme0n1p3
# mkfs.ext4 /dev/nvme0n1p4  
## mount the partition
# mount /dev/nvme0n1p3 /mnt
# mkdir /mnt/{boot,home}
# mount /dev/nvme0n1p1 /mnt/boot
# mount /dev/nvme0n1p4 /mnt/home
# Installation
## select the mirrors
# pacman -Sy pacman-contrib
# cp /etc/pacman.d/mirrorlist{,.backup}
# cp /etc/pacman.d/mirrorlist{,.tmp}
# sed -i 's/^Server/#Server/' /etc/pacman.d/mirrorlist.tmp
edit with vim, `mirrorlist.tmp` file to uncomment the countries to test for speed, for example:
:%s/Japan\n#/Japan\r/g
uncomment for the following countries:
* South Korea
* Japan
* Taiwan
** tip: do not uncomment `kaist` server, it is not a proper mirror server
# rankmirrors -n 5 /etc/pacman.d/mirrorlist.tmp > /etc/pacman.d/mirrorlist
## install base package
# pacstrap /mnt base base-devel vim wpa_supplicant
** `wpa_supplicant`: for wifi connection using `netctl`
# Configure the system
before going further let's copy the configuration files we used
# cp {,/mnt}/etc/netctl/wifi_1102
# cp {,/mnt}/etc/pacman.d/mirrorlist.tmp
# cp {,/mnt}/etc/pacman.d/mirrorlist.backup
## Fstab
# genfstab -U /mnt >> /mnt/etc/fstab
## chroot
# arch-chroot /mnt
## timezone
# ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
# hwclock --systohc
## localization
uncomment `en_US.UTF-8 UTF-8` and other needed locales in `/etc/locale.gen`, and generate them with:
# locale-gen
in my case, I uncomment `en_US.UTF-8 UTF-8` only.
## network configuration
# echo sy-arch > /etc/hostname
edit `/etc/hosts`
127.0.0.1    localhost
::1          localhost
127.0.1.1    sy-arch.localdomain    sy-arch
## set root password and create a regular user
# passwd
# useradd -m -g users -G wheel -s /bin/bash syoh
# passwd syoh
# visudo
then uncomment the following
# %wheel ALL=(ALL) ALL
## boot loader
# bootctl install
# vim /boot/loader/loader.conf
delete all contents and add the following
default arch
timeout 2
then create and edit `/boot/loader/entries/arch.conf`
and write down the following and save:
title Arch Linux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options root=
what we need is `PARTUUID` of the root partition
to know the `PARTUUID` we need to use command `blkid`
since it is very long and complicated string for human, it's better to copy and paste it.
we can insert the output of blkid 
# blkid >> /boot/loader/entries/arch.conf
and then copy and paste using editor
OR, in alternative and better way, we can use vim's command
:r !blkid
then yank and paste:
PARTUUD="xxxxxxxx-......"
the completed contents should look like
title Arch Linux
linux /vmlinuz-linux
initrd /initramfs-linux.img
options root=PARTUUD=xxxxxxxx-...... rw
## reboot
# exit
# umount -R /mnt
# reboot
# Post installation
## internet connection
get the name of wireless device's interface
$ ip link
it's same old `wlp2s0`.
let's use the conf file we copied
$ sudo netctl start wifi_1102
## install microcode fix for intel processor
$ sudo pacman -S intel-ucode
## install Xorg
$ sudo pacman -S xorg xf86-input-libinput
** `xf86-input-libinput`: libraries for input devices
install it using default options
## install bumblebee
bumblebee: mimic nvidia optimus
check graphics devices and install latest nvidia driver
$ lspci -k | grep -EA2 "(VGA|3D)"
can identify "intel UHD graphics 620" and "Geforce MX150"
install `bumblebee` and required packages
$ sudo pacman -S bumblebee mesa nvidia xf86-video-intel
add user to `bumblebee` group and enable `bumblebeed.service`
$ sudo gpasswd -a syoh bumblebee
$ sudo systemctl enable bumblebeed.service
## install display manager: lightdm
use `lightdm`
$ sudo pacman -S lightdm lightdm-gtk-greeter
set the default greeter to `lightdm-gtk-greeter`
by modifying `/etc/lightdm/lightdm.conf`
[Seat:*]
...
greeter-session=lightdm-gtk-greeter
...
enable lightdm on systemd
$ sudo systemctl enable lightdm.service
## Xorg configuration check
make skeleton for xorg.conf
$ sudo Xorg :0 -configure
edit touchpad configuration
$ sudo ln -s /usr/share/X11/xorg.conf.d/40-libinput.conf /etc/X11/xorg.conf.d/40-libinput.conf
and edit `40-libinput.conf` file by following command and add the red-faced lines:
$ sudo vim /etc/X11/xorg.conf.d/40-libinput.conf
------------------------------------------------
...
Section "InputClass"
    Identifier "libinput touchpad catchall"
    ...
    Option "NaturalScrolling" "true"
    Option "Tapping" "on"
    ...
