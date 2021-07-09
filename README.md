# Install arch linux

> Check disks state

```
lsblk
```

> Clear all partitions

```
gdisk /dev/{disk_name} (for example /dev/sda)
x
z
y
y
```
