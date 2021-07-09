# Install arch linux

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

> Create partitions for `root` `boot` `var` `home` directories (for swap partition use Linux swap / Solaris type [82])

```
example:
fdisk /dev/{disk_name}

fdisk /dev/sda
boot = 512M (SSD)
var = 512M (SSD)
swap = 8192M (SSD)
root = remaining free space (SSD)

fdisk /dev/sdb
home = remaining free space (HDD)
```

> Format the partitions

```
example:
mkfs.btrfs /dev/{partition_name}

mkfs.btrfs /dev/sda1
mkfs.btrfs /dev/sda2
mkswap /dev/sda3
swapon /dev/sda3
mkfs.btrfs /dev/sda4
mkfs.btrfs /dev/sdb1
```

> Mount the partitions

```
example:
mount /dev/{partition_name} /mnt/{directory_name}

mount -o noatime,compress=zstd,space_cache,discard=async /dev/sda4 /mnt
mkdir /mnt/{boot,var,home}
mount /dev/sda1 /mnt/boot
mount -o noatime,compress=zstd,space_cache,discard=async /dev/sda2 /mnt/var
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

> Config the pc

```
arch-chroot /mnt
nano /etc/locale.gen (uncomment en_US.UTF-8)
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
ln -sf /usr/share/zoneinfo/{country}/{city} /etc/localtime
hwclock --systohc --utc
echo {pc_name} > /etc/hostname
passwd
useradd -m -g users -G wheel -s /bin/bash {username}
passwd {username}
nano /etc/sudoers (uncomment %wheel ALL=(ALL) ALL)
```

> Set autostart

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
