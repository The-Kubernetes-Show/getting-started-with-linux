# Chapter 10: Disk and File System Management

---

[YouTube Video](https://youtu.be/3HA16AIIeFk)

---

Linux exposes storage through a single directory tree. In this chapter you will see how disks and partitions fit into that model and how to manage them safely as a beginner.

## 10.1 Learning goals

By the end of this chapter you should be able to:

- Explain the relationship between disks, partitions, and file systems  
- Compare MBR and GPT partition tables at a high level  
- Inspect disks, partitions, and file systems from both GUI and CLI  
- Create a simple ext4 filesystem, mount it, and make it persistent with `/etc/fstab`  
- Understand basic NFS mount options and how to mount with systemd  
- Recognize advanced filesystems such as XFS and Btrfs and when they are used

---

## 10.2 Core concepts

### 10.2.1 Disks, partitions, and filesystems

At the lowest level you have disks. These can be physical drives or virtual disks in a hypervisor or cloud. A disk is usually represented as a block device such as `/dev/sda` or `/dev/vda`.

You normally split a disk into one or more partitions. Each partition is a section of the disk that can hold a filesystem and appears as `/dev/sda1`, `/dev/sda2`, and so on. Ubuntu’s “Manage volumes and partitions” page describes this model from a desktop point of view:  
https://help.ubuntu.com/stable/ubuntu-help/disk-partitions.html.en

A filesystem defines how data is stored on a partition. ext4 and XFS are common Linux filesystems. The generic filesystem man page (`man 5 fs` or the online version at https://manpages.ubuntu.com/manpages/trusty/man5/fs.5.html) lists many supported filesystem types and how the kernel interacts with them.

When a filesystem is mounted it becomes part of the single directory tree, attached at a mount point such as `/`, `/home`, or `/mnt/data`.

> **Hands on**  
> - Run:
>   ```bash
>   ls /dev/sd* /dev/vd* 2>/dev/null
>   ```
>   to see disk devices on your system  
> - Run:
>   ```bash
>   mount |grep ^/dev|head
>   ```
>   and identify a few mount points and their filesystem types  

---

### 10.2.2 MBR vs GPT partition tables

Every disk with partitions uses a partition table that describes where those partitions live. The two common formats are MBR and GPT.

- **MBR (Master Boot Record)** stores partition information in the first sector of the disk and traditionally supports up to four primary partitions (or three primary plus one extended). MBR is a classic method to save partition information on a hard disk. It resides on the first sector of the boot disk. It stores disk partition information and a bootloader program. When we start a system, the BIOS scans all hard disks, detects the presence of MBR, loads the bootloader program in RAM from the default boot disk, executes the boot code to read the partition table, identifies the /boot partition, loads the kernel in RAM, and passes control over to it. MBR supports three types of partition: primary, extended, and logical on a single disk. We can use only primary and logical partitions for data storage. We cannot use the extended partition for data storage. It stores logical partitions. Technically, MBR supports only four primary partitions numbered from 1 to 4. MBR supports a maximum of 14 partitions (11 logical partitions). 
![MBR](/static/images/mbr-hard-disk-extended.png)


- **GPT (Globally Unique Identifiers Partition Table a.ka. GUID GPT)** is part of the **Unified Extensible Firmware Interface (UEFI)** specification and supports large disks, many partitions, and redundant partition table copies for improved robustness. GUID Partition Table (GPT) is a modern way to save partition information. It was developed to overcome the limitations of MBR. MBR is a classic way to save partition information. MBR only supports hard disks up to 2 TB. With the increasing use of disks larger than 2TB, a new partitioning standard called GUID (Globally Unique Identifiers) was developed and integrated into the UEFI firmware. It is called GPT (GUID Partition Table). It uses a 4KB sector to store partition information and bootloader. It allows us to create up to 128 partitions. Since it supports too many partitions by default, it does not use the concept of primary, logical, and extended partitions. It also saves a copy of the partition information before the end of the disk for redundancy. These features are not available in MBR. MBR stores partition information and the bootloader program in the first sector of the hard disk. It uses a 512 bytes sector. Due to this, it can store information about a maximum of 14 partitions and 2 TB of disk space. To address this issue, GPT uses a sector of 4KB. 4KB sector space allows GPT to store information about up to 128 partitions, and each partition can have a maximum of 18 exabytes of space. The MBR is non-redundant. It stores information only in a single place. It means if the first sector corrupts, the disk becomes useless. To solve this issue, GPT stores a copy of the partition information before the end of the disk for redundancy.
![GPT](/static/images/gpt-disk-layout.png)

The Arch “Partitioning” article has a good neutral summary of how these schemes look from a Linux point of view:  https://wiki.archlinux.org/title/Partitioning

Modern Linux installers tend to use GPT by default on UEFI based systems. Disk tools such as `fdisk` and `parted` let you choose which partition table to use when initializing a disk, which is a common task before creating partitions.

> **Hands on**  
> - Pick a non removable disk (for example `/dev/sda`) and run:
>   ```bash
>   sudo parted /dev/sda print
>   ```
>   Look for the `Partition Table` line and note whether it is `msdos` (MBR) or `gpt`

---

### 10.2.3 Devices and mount points

Linux exposes storage devices as block devices under `/dev`. Partitions are typically numbered so `/dev/sda1` is the first partition on the first SCSI or SATA disk.

The system mounts these devices into the directory tree at mount points. Root `/` is usually on the first partition of the first disk, while `/home` or `/boot` may be on separate partitions depending on how the system was installed. Ubuntu’s disks and partitions help talks about this layout:  
https://help.ubuntu.com/stable/ubuntu-help/disk-partitions.html.en

The `/etc/fstab` file provides static information about filesystems so they can be mounted automatically at boot or on demand. See `man 5 fstab` or the online man page:  
https://manpages.ubuntu.com/manpages/noble/man5/fstab.5.html

> **Hands on**  
> - Run:
>
>   ```bash
>   lsblk
>   ```
>
>   to see the mapping between devices, partitions, and mount points  
> - View `/etc/fstab` with:
>
>   ```bash
>   cat /etc/fstab
>   ```
>
>   and try to match each line to a device and mount point from `lsblk`

---

## 10.3 Inspecting disks and storage

### 10.3.1 Using Ubuntu Disks utility

On Ubuntu desktop the Disks utility gives a friendly view of your storage layout. The official documentation is here:  
https://help.ubuntu.com/stable/ubuntu-help/disk.html.en  
https://help.ubuntu.com/stable/ubuntu-help/disk-partitions.html.en

Typical tasks in Disks:

- View all disks and their partitions, including size, filesystem type, and mount point  
- Inspect a partition to see details such as UUID and flags before making changes  

> **Hands on**  
> - Open the Disks utility from your application menu  
> - Identify your system disk and list:
>   - how many partitions it has  
>   - what filesystems they use  
>   - which partition is mounted as `/`  
>   Use the information panel for each partition to see the filesystem type and UUID

---

### 10.3.2 Inspecting from the command line

For servers or headless systems you spend most of your time on the command line. RHEL’s “Managing storage devices” guide covers the main tools:  
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_storage_devices/index

Some useful commands:

- `lsblk` shows block devices, sizes, hierarchy, and mount points  
- `lsblk -f` adds filesystem type, label, and UUID which is very useful for `/etc/fstab`  
- `df -h` shows filesystem usage for mounted filesystems in human readable format  
- `sudo fdisk -l` lists partition tables and partitions for all disks

> **Hands on**  
> - Run:
>
>   ```bash
>   lsblk -f
>   df -h
>   ```
>
> - For each mounted filesystem shown in `df -h` try to find the corresponding device and filesystem type in `lsblk -f`  
> - Note the UUID for your root filesystem from `lsblk -f`. You will reuse this when learning about `/etc/fstab`

---

### 10.3.3 Checking partition table type

Before changing partitions it helps to know how the disk is partitioned. The `parted` tool prints this along with other details:

```bash
sudo parted /dev/sda print
```

Look for a line like:

```text
Partition Table: gpt
```

or:

```text
Partition Table: msdos
```

This tells you whether the disk is using GPT or MBR which can influence how many partitions you can create and how boot loaders are configured.

---

## 10.4 Creating a simple partition and filesystem

In this section you will walk through a simple workflow similar to the basic disk administration tasks in the RHEL 9 storage documentation. Only perform these steps on a test virtual machine or an empty disk to avoid data loss.

RHEL 9 storage docs PDF:
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/pdf/managing_storage_devices/Red_Hat_Enterprise_Linux-9-Managing_storage_devices-en-US.pdf

### 10.4.1 Discovering a new disk

Attach a second virtual disk to your VM. Cloud and virtualization platforms often let you add a new disk that appears as `/dev/sdb` or `/dev/vdb`.

Check that the disk is visible and unpartitioned:

```bash
lsblk
sudo fdisk -l /dev/sdb
```

You should see the device with no listed partitions under it.

---

### 10.4.2 Creating a partition with fdisk

Red Hat documentation includes `fdisk` as a tool for creating and modifying partitions on MBR or GPT disks.

Example workflow for `/dev/sdb`:

```bash
sudo fdisk /dev/sdb
```

Inside `fdisk`:

- Type `n` to create a new partition and accept the defaults for a single full disk partition
- Type `p` to print the partition table and verify the new partition
- Type `w` to write changes and exit

After exiting run:

```bash
lsblk
```

You should now see `/dev/sdb1` under `/dev/sdb`.

> **Hands on**
> - Use this procedure to create exactly one partition on your test disk
> - Confirm the partition type and size match your expectations in `lsblk` and `fdisk -l`

---

### 10.4.3 Creating an ext4 filesystem

The ext4 filesystem is a widely used journaling filesystem. The Linux kernel ext4 documentation is here:
https://docs.kernel.org/admin-guide/ext4.html

Create an ext4 filesystem on `/dev/sdb1`:

```bash
sudo mkfs.ext4 /dev/sdb1
```

Then verify:

```bash
lsblk -f
```

You should see `ext4` in the `FSTYPE` column for `/dev/sdb1` and a UUID assigned to the filesystem.

> **Hands on**
> - Capture the UUID from `lsblk -f` and save it somewhere for the next steps
> - Create a directory `/mnt/data` that will act as a mount point:
>
>   ```bash 
>   sudo mkdir -p /mnt/data 
>   ```

---

### 10.4.4 Mounting the filesystem

Mount the new filesystem:

```bash
sudo mount /dev/sdb1 /mnt/data
```

Confirm it is mounted:

```bash
df -h | grep /mnt/data
```

You should see a line showing `/dev/sdb1` mounted on `/mnt/data` with its size and usage.

> **Hands on**
> - Create a test file:
>   ```bash 
>   sudo touch /mnt/data/hello.txt 
>   sudo ls -l /mnt/data 
>   ```
>
> - Unmount and remount:
>
>   ```bash 
>   sudo umount /mnt/data
>   ls -l /mnt/data 
>   # Notice the file is missing because 
>   # File was stored in /dev/sdb1 and we unmounted it.
>   sudo mount /dev/sdb1 /mnt/data 
>   ls /mnt/data 
>   ```
>
>   Confirm that `hello.txt` is still present which shows that data lives on the filesystem, not the mount point

---

### 10.4.5 Persistent mounts with /etc/fstab

The `/etc/fstab` file contains static information about filesystems so that they can be mounted automatically. See:

- `man 5 fstab` or
- Online: https://manpages.ubuntu.com/manpages/noble/man5/fstab.5.html
- Ubuntu wiki explanation: https://wiki.ubuntu.com/FSTAB

A generic line in `/etc/fstab` uses this structure:

```text
<fs_spec>  <mountpoint>  <type>  <options>  <dump>  <pass>
```

For your new ext4 filesystem you can use the UUID from `lsblk -f` which is the recommended approach:

```text
UUID=<uuid-of-sdb1>  /mnt/data  ext4  defaults  0  2
```

To apply the new entry without rebooting:

```bash
sudo mount -a
```

The `mount` command reads `/etc/fstab` and mounts anything that is not yet mounted.

> **Hands on**
> - Edit `/etc/fstab` with:
>   ```bash 
>   sudo nano /etc/fstab 
>   ```
>   or your preferred editor
> - Add a line for your `/mnt/data` ext4 filesystem using its UUID
> - Run:
>   ```bash 
>   sudo mount -a 
>   df -h | grep /mnt/data 
>   ```
>
> - Reboot the VM and verify that `/mnt/data` is still mounted automatically

---

## 10.5 NFS and network filesystem mounts

NFS lets a system mount a remote directory over the network. The NFS man page documents how NFS entries look in `/etc/fstab`:

- `man 5 nfs` or
- Online: https://manpages.ubuntu.com/manpages/focal/en/man5/nfs.5.html

RHEL also has a chapter on mounting NFS shares:
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_file_systems/mounting-nfs-shares_managing-file-systems

### 10.5.1 Basic NFS fstab entry

An NFS mount in `/etc/fstab` looks like this:

```text
<nfs_server>:/export/path  /mnt/nfs  nfs  defaults  0  0
```

For example:

```text
nfs.example.com:/srv/export  /mnt/nfs  nfs  defaults  0  0
```

The NFS man page describes how each field works and what defaults are used.

### 10.5.2 Common NFS options

Some of the most important options from `man 5 nfs`:

- `nfsvers=` selects the NFS version, for example `nfsvers=4`
- `rsize=` and `wsize=` control read and write buffer sizes
- `hard` or `soft` defines how the client behaves when the server is not responding
- `timeo=` sets the timeout in tenths of a second for NFS operations

Example:

```text
nfs.example.com:/srv/export  /mnt/nfs  nfs  nfsvers=4,hard,rsize=65536,wsize=65536,timeo=600  0  0
```

> **Hands on**
> - If you have access to an NFS server in a lab:
>   - Create `/mnt/nfs` on your client
>   - Add an `/etc/fstab` entry using `nfsvers=4`
>   - Run:
>
>     ```bash 
>     sudo mount -a 
>     df -h | grep /mnt/nfs 
>     ```
> - Experiment by temporarily switching between `soft` and `hard` on a test mount and observe behavior if the server is briefly unavailable

---

## 10.6 Mounting filesystems with systemd

Modern Linux systems use systemd to manage services and mounts. RHEL’s “Managing file systems” guide has a chapter on mounting file systems on demand and using `systemd.automount`:
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/managing_file_systems/mounting-file-systems-on-demand_managing-file-systems

### 10.6.1 systemd and fstab

systemd creates mount units at boot based on `/etc/fstab`. Adding an option such as `x-systemd.automount` lets systemd turn a static mount into an on demand automount.

For example:

```text
UUID=<uuid-of-sdb1>  /mnt/data  ext4  defaults,x-systemd.automount  0  2
```

RHEL’s “Using systemd.automount to mount a file system on demand with /etc/fstab” section describes the full flow.

> **Hands on**
> - Add `x-systemd.automount` to your `/mnt/data` entry in `/etc/fstab`
> - Run:
>
>   ```bash 
>   sudo systemctl -t mount list-unit-files
>   sudo systemctl daemon-reload 
>   sudo systemctl restart local-fs.target 
>   sudo systemctl -t mount list-unit-files
>   ```
>
> - Check:
>
>   ```bash 
>   systemctl status mnt-data.automount 
>   ```
>
> - Run `ls /mnt/data` to trigger the mount and then check `df -h`

---

### 10.6.2 systemd .mount units

systemd can manage mounts entirely through unit files. The same RHEL chapter has a section “Using systemd.automount to mount a file system on-demand with a mount unit” that walks through this.

A mount unit is named after the mount point with slashes replaced by dashes, so a mount point `/mnt/data` uses the unit `mnt-data.mount`.

Example unit file `/etc/systemd/system/mnt-data.mount`:

```text
[Unit]
Description=Data mount

[Mount]
What=/dev/disk/by-uuid/<uuid-of-sdb1>
Where=/mnt/data
Type=ext4
Options=defaults

[Install]
WantedBy=multi-user.target
```

Then:

```bash
sudo systemctl -t mount list-unit-files
sudo systemctl daemon-reload
sudo systemctl enable --now mnt-data.mount
sudo systemctl -t mount list-unit-files
```

> **Hands on**
> - Comment out your `/mnt/data` line in `/etc/fstab`
> - Create `mnt-data.mount` as above using the UUID from `lsblk -f`
> - Enable it with `systemctl` and verify the mount with:
>
>   ```bash 
>   df -h | grep /mnt/data 
>   ```
>
> - Inspect the unit with:
>
>   ```bash 
>   systemctl status mnt-data.mount 
>   ```

---

## 10.7 Advanced filesystems (preview)

Linux supports many filesystems beyond ext4. The generic filesystem man page lists types such as XFS, Btrfs, and others along with references to their specific documentation:
https://manpages.ubuntu.com/manpages/trusty/man5/fs.5.html

Red Hat documentation describes XFS as a high performance filesystem suitable for large files and parallel I/O and it is the default filesystem in some RHEL configurations:
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9

Btrfs, available on some distributions, offers snapshotting, built in checksums for data and metadata, and other advanced features that are documented in its own man pages.

You can usually choose these filesystems when creating them with tools such as `mkfs.xfs` or `mkfs.btrfs`, and they can then be mounted like any other filesystem using `/etc/fstab` or systemd units.

> **Hands on (optional)**
> - On a test disk create an XFS filesystem:
>
>   ```bash 
>   sudo mkfs.xfs /dev/sdb1 
>   ```
>
>   then mount it at `/mnt/xfsdata` and add a corresponding `/etc/fstab` line using its UUID
> - Explore the output of:
>
>   ```bash 
>   df -T 
>   ```
>
>   to see filesystem types for each mount

---

## 10.8 Homework

To really internalize this chapter, work through these exercises on a disposable VM or lab environment.

1. **Map your system layout**
    - Use `lsblk -f`, `df -h`, and `/etc/fstab` to draw a simple diagram of your system showing:
>     - disks
>     - partitions
>     - filesystems
>     - mount points
    - For each disk, use `sudo parted /dev/<disk> print` to label it as MBR or GPT
2. **Design a multi mount layout**
    - Add a second test disk
    - Create two partitions on it and format one as ext4 and one as XFS (if XFS is available on your distro)
    - Mount them at `/data/app` and `/data/logs` using `/etc/fstab` and verify they mount after reboot
3. **Experiment with NFS**
    - Set up or use an existing NFS export in a lab
    - Mount it on a client with a simple `/etc/fstab` entry first
    - Extend the options with `nfsvers`, `hard`, and `timeo` and write down how behavior changes if you stop the NFS service on the server temporarily
4. **Try a systemd only mount**
    - Remove a non critical entry from `/etc/fstab` and replace it with a `.mount` unit managed by systemd
    - Enable it and confirm it mounts at boot and can be controlled with `systemctl start` and `systemctl stop`
5. **Research one advanced filesystem**
    - Using only official documentation and man pages, write a one page note for yourself about one advanced filesystem such as XFS or Btrfs
    - Capture:
>     - its main strengths
>     - typical use cases
>     - one risk or limitation to be aware of

If you can complete these tasks and explain what you did to someone else, you are ready for more advanced topics like LVM, RAID, and snapshot based backups.