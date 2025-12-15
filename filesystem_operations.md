# Filesystem Troubleshooting – Linux Practical Guide

This file complements `disk.md` and **includes ALL practical commands you provided**, organized and normalized.
Focus: **listing, mounting, unmounting, ejecting, and fixing disks**.

---

## List information about block devices

```bash
# Think of it like: "See all disks, partitions, and filesystems"
lsblk -f
```

Example output:
```text
NAME        FSTYPE   FSVER LABEL       UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
sda                                                                                         
└─sda1      vfat     FAT32 CCCOMA_X64F CC0A-ADBD                               3.5G    76% /media/tchedem/CCCOMA_X64F
```

---

## Unmount and detach a disk (udisks – legacy & modern)

### Using `udisks`

```bash
# Think of it like: "Unmount by device"
udisks --unmount /dev/sdXY

# Think of it like: "Physically detach the disk"
udisks --detach /dev/sdX
```

---

### Using `udisksctl` (recommended)

```bash
# Think of it like: "Unmount a specific partition"
udisksctl unmount -b /dev/sdb1

# Think of it like: "Safely power off / eject the USB device"
udisksctl power-off -b /dev/sdb
```

---

### Using mount path (when device name is unknown)

```bash
# Think of it like: "Unmount using the mount directory"
udisks --unmount /media/tchedem/CCCOMA_X64F

udisksctl unmount /media/tchedem/CCCOMA_X64F
```

---

## Simpler / classic tools

```bash
# Think of it like: "Classic unmount"
sudo umount /media/tchedem/CCCOMA_X64F

# Think of it like: "Classic eject"
sudo eject /dev/sdb
```

---

## Mount a disk manually

```bash
# Think of it like: "Attach a disk with full read/write permissions"
sudo mount -o rw,umask=000 /dev/sda1 /mnt/backup_plus
```

---

## Device is busy (cannot unmount)

### Symptom
```bash
umount: target is busy
```

### Identify blocking processes

```bash
# Think of it like: "Who is using the disk?"
lsof +f -- /media/tchedem/CCCOMA_X64F
```

```bash
# Think of it like: "Which process blocks the mount?"
fuser -m /media/tchedem/CCCOMA_X64F
```

---

## Force unmount (last resort)

```bash
# Think of it like: "Disconnect now, fix later"
sudo umount -l /media/tchedem/CCCOMA_X64F
```

---

## Disk mounted read-only

```bash
# Think of it like: "Remount disk as read-write"
sudo mount -o remount,rw /dev/sda1 /mnt/backup_plus
```

---

## Permission denied on mounted disk

```bash
# Think of it like: "Check ownership"
ls -ld /mnt/backup_plus
```

```bash
# Think of it like: "Give ownership to current user"
sudo chown -R $USER:$USER /mnt/backup_plus
```

---

## Disk not visible

```bash
# Think of it like: "List block devices"
lsblk
```

```bash
# Think of it like: "Identify disks by UUID and filesystem"
sudo blkid
```

---

## Filesystem check

```bash
# Think of it like: "Scan and repair filesystem errors"
sudo fsck /dev/sda1
```

---

## Emergency safe-removal checklist

```bash
# Think of it like:
# lsblk                     → identify device
# udisksctl unmount -b ...  → unmount safely
# udisksctl power-off -b .. → power off disk
# umount / eject            → fallback tools
# fsck                      → repair if needed
```

