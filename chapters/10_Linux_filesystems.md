# Disk and File System Management in Linux

In this session, we’re going to explore one of the most important topics in Linux system administration: **disk and file system management**. By the end, you’ll understand how to check disk usage, mount and unmount drives, and get a basic grasp on partitions and filesystems.  

Let’s dive in.

---
## Different types of filesystems

+-------------------------------+-----------+---------------------------------------------------------------------+
| Type                          | Name      | Typical Usage / Notes                                               |
+-------------------------------+-----------+---------------------------------------------------------------------+
| Disk or local FS              | XFS       | Default in RHEL 9; high performance, great for large filesystems    |
|                               | ext4      | Evolved from ext2/ext3; compatible, robust, slightly lower max size |
+-------------------------------+-----------+---------------------------------------------------------------------+
| Network or client-server FS   | NFS       | Share files across Linux/Unix systems over LAN                      |
|                               | SMB       | File sharing with Windows systems                                   |
+-------------------------------+-----------+---------------------------------------------------------------------+
| Shared storage FS             | GFS2      | Clustered file system, shared disk/write access for clusters        |
+-------------------------------+-----------+---------------------------------------------------------------------+
| Volume-managing FS            | Stratis   | Volume management on top of XFS/LVM; Btrfs/ZFS-like features        |
+-------------------------------+-----------+---------------------------------------------------------------------+
| Special-purpose FS            | tmpfs     | Temporary file storage in RAM; fast but non-persistent              |
|                               | cgroup2   | Control groups for resource management                              |
|                               | procfs    | Virtual filesystem for process and kernel info                      |
|                               | sysfs     | Virtual filesystem for device and kernel info                       |
+-------------------------------+-----------+---------------------------------------------------------------------+


## Checking Disk Usage

The first step in managing storage is knowing how much space is being used and where.

To see the overall disk usage of your mounted filesystems, use:

```bash
df -h
```

The `-h` flag shows results in a human-readable format (GB, MB, etc.). This command tells you the **size**, **used space**, **available space**, and **mount point** for each filesystem.

If you want to check how much space specific directories are using, use `du`:

```bash
du -sh /var/log
du -h --max-depth=1 /home
```

- `du -sh /var/log` → shows the total size of `/var/log`.
- `du -h --max-depth=1 /home` → breaks down the storage usage of directories directly under `/home`.

### References

- Ubuntu manual: [df](https://manpages.ubuntu.com/manpages/noble/en/man1/df.1.html)  
- Red Hat docs: [Managing file systems](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_file_systems/index)

---

## Mounting and Unmounting Drives

In Linux, filesystems don’t just “appear.” They need to be *mounted* to a directory (known as a mount point) before you can use them.

On Linux, UNIX, and similar operating systems, file systems on different partitions and removable devices (CDs, DVDs, or USB flash drives for example) can be attached to a certain point (the mount point) in the directory tree, and then detached again. While a file system is mounted on a directory, the original content of the directory is not accessible.

Note that Linux does not prevent you from mounting a file system to a directory with a file system already attached to it.

When mounting, you can identify the device by:

- a universally unique identifier (UUID): for example, ***UUID=34795a28-ca6d-4fd8-a347-73671d0c19cb***
- a volume label: for example, ***LABEL=home***
- a full path to a non-persistent block device: for example, ***/dev/sda3***

When you mount a file system using the mount command without all required information, that is without the device name, the target directory, or the file system type, the mount utility reads the content of the `/etc/fstab` file to check if the given file system is listed there. The `/etc/fstab` file contains a list of device names and the directories in which the selected file systems are set to be mounted as well as the file system type and mount options. Therefore, when mounting a file system that is specified in `/etc/fstab`, the following command syntax is sufficient:

- Mounting by the mount point:

```bash
sudo mount /mnt
```

- Mounting by the device name:

```bash
sudo mount /dev/sdb1
```

To see what’s currently mounted:

```bash
mount
```

Or, for a cleaner view:

```bash
findmnt
```

Suppose you have a new partition at `/dev/sdb1`, and you want to make it available under `/mnt`:

```bash
sudo mount /dev/sdb1 /mnt
```

Once you’re done using it, you should unmount it:

```bash
sudo umount /mnt
```

To list your block devices (useful for finding disks and partitions):

```bash
lsblk
```

This gives you an overview of disks, partitions, sizes, and mount points.

### References
- Ubuntu manual: [mount](https://manpages.ubuntu.com/manpages/noble/en/man8/mount.8.html)  
- Red Hat docs: [Mounting file systems](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/managing_file_systems/mounting-file-systems_managing-file-systems)

---

## Partitions and Filesystems

Now that you know how to check usage and mount drives, let’s go a bit deeper into **partitions** and **filesystems**.

- **Partitions** are divisions of a disk.  
- **Filesystems** are the data structures (like ext4, xfs) that organize files on those partitions.

To inspect your partitions and the filesystems on them:

```bash
lsblk -f
```

You’ll see filesystem types, labels, and mount points.

To see detailed disk partition layouts:

```bash
sudo fdisk -l
```

If you add a new partition, you’ll need to format it before you can use it. For example, to format `/dev/sdb1` with the ext4 filesystem:

```bash
sudo mkfs.ext4 /dev/sdb1
```

**⚠️ Warning:** Only run this on disks you are okay with erasing—this will destroy all data on the partition.

### References
- Ubuntu manual: [mkfs](https://manpages.ubuntu.com/manpages/noble/en/man8/mkfs.8.html)  
- Red Hat docs: [Creating file systems](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_file_systems/assembly_creating-an-xfs-file-system_managing-file-systems)

---

## Homework

Try these tasks on your system (or a safe virtual machine):

1. Run `df -h` and find:
   - Which partition is mounted on `/`
   - How much free space is left there

2. Use `du -sh` to find the size of your `/home` directory. Compare your result with `df -h` and note the difference.

3. Run `lsblk` to list all available disks and partitions. Which partitions are currently mounted?

4. (Optional, best on a VM to avoid risks):  
   - Create a virtual disk and attach it to your VM.  
   - Use `lsblk` to identify it.  
   - Format it with ext4 using `mkfs.ext4`.  
   - Mount it under `/mnt/testdisk`, create some files inside, and then unmount it.  

---

## Additional Resources

For more details, check out official documentation:
- Red Hat File System Management Guide: [Managing File Systems](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/managing_file_systems/)  


