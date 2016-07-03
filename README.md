Arch Installation
===

Partition your disks
---

1. EFI System Partition
    - 512MB
    - FAT32
        >mkfs.fat -F32 /dev/sdxY

2. Root Partition
    - Choose whatever file system you want, BUT if you choose to make it
        BTRFS you will need a separate partition for your swap since
        BTRFS doesn't play well with swap files.

    - If you want different file systems on different partitions you can
        choose to make separate partitions for /boot, /home, or any
        folder you choose. I don't recommend this, unless you need it,
        as it might create problems with storage later on.

    - I use LVM inside a LUKS volume, check the arch wiki if you want
    that.

Mount the Partitions
---
Mount the partitions you just made
```
# lsblk
# mount /dev/sdXY /mnt
```
Where X is the partition letter, and Y is the partition number of root


Pacstrap
---

##### Edit your mirrorlist

Put the mirror that is closest to you at the
top
```
# vim /etc/pacman.d/mirrorlist
```

##### Connect to the Internet
```
# wifi-menu
```

##### Pacstrap
```
# pacstrap /mnt base base-devel vim
```
> vim, because vim is awesome

Might take a while, depending on your download speed

Install Boot loader
---
I recommend grub, if you use something else that's on you.

##### CHROOT
```
# arch-chroot /mnt /bin/bash
```

##### Mount your EFI System partition
```
# mkdir /boot/esp
# mount /dev/sdXY /boot/esp
```

##### Install Grub
```
# grub-install --target=x86_64-efi --efi-directory=/boot/esp --bootloader-id=grub
# grub-mkconfig -o /boot/grub/grub.cfg
```

Post Installation
---
Follow the [Arch Wiki General recommendations](https://wiki.archlinux.org/index.php/General_recommendations)
