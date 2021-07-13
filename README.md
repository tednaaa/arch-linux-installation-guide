# Guide: how to install arch linux

> Check disks state

```
lsblk
```

> Clear all partitions

```
example:
gdisk /dev/{disk_name}
x
z
y
y

gdisk /dev/sda
gdisk /dev/sdb
```

> Create partitions for `root` `boot` `home` directories

```
example:
сfdisk /dev/{disk_name}

сfdisk /dev/sda
boot = 512M (SSD) [BIOS boot]
swap = 8192M (SSD) [Linux swap]
root = remaining free space (SSD) [Linux filesystem]

сfdisk /dev/sdb
home = remaining free space (HDD) [Linux filesystem]
```

> Format the partitions

```
example:
mkfs.btrfs /dev/{partition_name}

mkfs.btrfs /dev/sda1
mkswap /dev/sda2
swapon /dev/sda2
mkfs.btrfs /dev/sda3
mkfs.btrfs /dev/sdb1
```

> Mount the partitions

```
example:
mount /dev/{partition_name} /mnt/{directory_name}

mount -o noatime,compress=zstd,space_cache,discard=async /dev/sda3 /mnt
mkdir /mnt/{boot,home}
mount /dev/sda1 /mnt/boot
mount -o noatime,compress=zstd,space_cache,discard=async /dev/sdb1 /mnt/home
```

> Update packages and install base linux system

```
pacman -Syy
pacstrap /mnt base linux linux-firmware nano dhcpcd grub btrfs-progs
```

> Generate file system table and check

```
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab
```

> Chroot and set locales

```
arch-chroot /mnt

nano /etc/locale.gen (uncomment en_US.UTF-8)
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

> Set timezone

```
ln -sf /usr/share/zoneinfo/{country}/{city} /etc/localtime
hwclock --systohc --utc
```

> Set hostname

```
echo {pc_name} > /etc/hostname
passwd
```

> Add admin user

```
useradd -m -g users -G wheel -s /bin/bash {username}
passwd {username}
nano /etc/sudoers (uncomment %wheel ALL=(ALL) ALL)
```

> Enable autostart of dynamic host configuration protocol

```
systemctl enable dhcpcd
```

> Set up boot grub and reboot

```
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
mkinitcpio -p linux

exit
umount -R /mnt
reboot
```

> Install xorg

```
sudo pacman -S xorg xorg-xinit xorg-aps mesa-libgl
```

> Install drivers

```
Intel: sudo pacman -S xf86-video-intel
Nvidia: sudo pacman -S nvidia nvidia-settings
AMD: sudo pacman -S xf86-video-amdgpu
```

> Install xfce4 and sddm

```
sudo pacman -S xfce4 sddm
```

> Enable autostart of simple desktop display manager

```
systemctl enable sddm
```
