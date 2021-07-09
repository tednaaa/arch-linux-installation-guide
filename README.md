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

> Create partitions for `root`, `boot`, `var`, `home` directories (for swap partition use Linux swap / Solaris type [82])

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

mount /dev/sda4 /mnt
mkdir /mnt/{boot,var,home}
mount /dev/sda1 /mnt/boot
mount /dev/sda2 /mnt/var
mount /dev/sdb1 /mnt/home
```
