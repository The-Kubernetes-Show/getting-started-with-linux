# Getting Started with Linux on TKS

# Welcome to TKS (The Kubernetes Show)

## [Introduction with the Author](chapters/1_Introduction.md)

The following sequence is designed to progressively build your Linux skills, starting from the basics and advancing to more complex, hands-on topics. Each step builds on the previous material, ensuring a smooth learning curve aligned with the content in my videos and GitHub resources.

### Summary Table: Topics by Experience Level

| Level        | Topic Range         | Description                                  |
|--------------|--------------------|----------------------------------------------|
| Beginner     | 1–12               | Foundational Linux concepts and basic skills |
| Intermediate | 13–18              | Deeper system management and automation      |
| Advanced     | 19–29              | Kernel, security, performance, and scaling   |
| Specialized  | 30–33              | Developer tools, DevOps, and forensics       |

---

### Table of Contents

| Module | Topic                                    |GitHub Link | YouTube Link|
|--------|------------------------------------------|-----|-----|
| 1      | Introduction & Distributions |✅ [Link 1](chapters/2_introduction_to_linux.md)|✅ [Video - 27 Apr 2025](https://www.youtube.com/watch?v=XMQlhPtPryo)|
| 2      | Linux File System Structure |✅ [Link 1](chapters/3_linux_distros_and_our_first_linux_vm.md)|✅ [Video - 11 May 2025](https://www.youtube.com/watch?v=U3n230K5d1U)|
| 3      | Basic Linux Commands and Navigation              |✅ [Link 1](chapters/3.1_basic_linux_cmds.md) |✅ [Video - 26 May 2025](https://www.youtube.com/watch?v=GtxGxAdI_rI)|
| 4      | File Permissions and Ownership                   |✅ [Link 1](chapters/4_permissions_and_editors.md) |✅ [Video - 8 June 2025](https://www.youtube.com/watch?v=_pletZMbTT0)|
| 5      | Editing Files in Linux                           |✅ [Link 1](chapters/5_editing_files.md) |✅ [Video - 22 June 2025](https://www.youtube.com/watch?v=uLOZbbptePA)|
| 6      | Managing Software Packages                       |✅ [Link 1](chapters/6_software_package_management_in_linux.md) |✅ [Video - 13 July 2025](https://youtu.be/WdvJkZN3RFk)|
| 7      | User and Group Management                        |✅ [Link 1](chapters/7_user_and_group_management.md) |✅ [Video - 9 Aug 2025](https://www.youtube.com/watch?v=kWNYNjCiNvg)|
| 8      | Linux Processes and System Monitoring            |✅ [Link 1](chapters/8_Linux-rocesses-and-system-monitoring.md) |✅ [Video - 23 Aug 2025](https://youtu.be/nbGCAWTn8k8)|
| 9      | Networking Basics                                |                    |                    |
| 10     | Disk and File System Management                  |                    |                    |
| 11     | Shell Scripting Basics                           |                    |                    |
| 12     | System Logs and Troubleshooting                  |                    |                    |
| 13     | Advanced Shell Scripting                         |                    |                    |
| 14     | Job Scheduling and Automation                    |                    |                    |
| 15     | System Boot Process and GRUB                     |                    |                    |
| 16     | Process Management and Optimization              |                    |                    |
| 17     | Advanced File System Management                  |                    |                    |
| 18     | Backup and Restore Strategies                    |                    |                    |
| 19     | Linux Kernel Internals and Optimization          |                    |                    |
| 20     | Security and Hardening                           |                    |                    |
| 21     | Centralized Logging and Monitoring               |                    |                    |
| 22     | Networking: Advanced Configuration and Troubleshooting |                |                    |
| 23     | Virtualization and Containers                    |                    |                    |
| 24     | High Availability and Load Balancing             |                    |                    |
| 25     | Filesystem and Storage: Advanced Topics          |                    |                    |
| 26     | Performance Tuning and Troubleshooting           |                    |                    |
| 27     | Automation and Infrastructure as Code            |                    |                    |
| 28     | Cloud Integration and Hybrid Environments        |                    |                    |
| 29     | Documentation and Best Practices                 |                    |                    |
| 30     | Linux for Developers                             |                    |                    |
| 31     | Programming and Visualization Tools              |                    |                    |
| 32     | Linux in DevOps and CI/CD                        |                    |                    |
| 33     | Incident Response and Forensics                  |                    |                    |

---

##### 1. [Introduction to Linux, History of Linux and Video release plan](chapters/2_introduction_to_linux.md)

##### 2. [Getting Started with Linux: Major Linux Distributions and our first Linux VM (Virtual Machine)](chapters/3_linux_distros_and_our_first_linux_vm.md)

##### 3. [Basic Linux Commands and Navigation](chapters/3.1_basic_linux_cmds.md)

- Learn to use the terminal/CLI (Command Line Interface)
- Navigating the file system (`cd`, `ls`, `pwd`)
- Viewing and manipulating files (`cat`, `less`, `head`, `tail`, `cp`, `mv`, `rm`, `mkdir`, `rmdir`)

##### 4. [File Permissions and Ownership](chapters/4_permissions_and_editors.md)

- Understanding permissions (read, write, execute)
- Changing permissions with `chmod`
- Changing ownership with `chown` and `chgrp`
- The significance of root and regular users

##### 5. [Editing Files in Linux](chapters/5_editing_files.md)

- Introduction to text editors (`nano`, `vim`, `gedit`)
- Basic editing, saving, and exiting

##### 6. [Managing Software Packages](chapters/6_software_package_management_in_linux.md)

- Installing, updating, and removing software using package managers (`apt`, `yum`, `dnf`, `zypper`)
- Searching for packages
- Understanding repositories

##### 7. [User and Group Management](chapters/7_user_and_group_management.md)

- Creating and managing users (`useradd`, `usermod`, `passwd`)
- Creating and managing groups (`groupadd`, `groupmod`)
- Switching users (`su`, `sudo`)

##### 8. [Linux Processes and System Monitoring](chapters/8_Linux-rocesses-and-system-monitoring.md)

- Viewing running processes (`ps`, `top`, `htop`)
- Managing processes (`kill`, `pkill`, `jobs`, `bg`, `fg`)
- Understanding system resource usage

##### 9. Networking Basics

- Checking network configuration (`ifconfig`, `ip`, `hostname`)
- Testing connectivity (`ping`, `traceroute`, `netstat`)
- Editing network configuration files

##### 10. Disk and File System Management

- Checking disk usage (`df`, `du`)
- Mounting and unmounting drives (`mount`, `umount`)
- Understanding partitions and filesystems

##### 11. Shell Scripting Basics

- Introduction to shell scripts
- Writing and running simple scripts
- Using variables and basic control structures (if, for, while)

##### 12. System Logs and Troubleshooting

- Locating and reading log files (`/var/log/`)
- Using `dmesg` and `journalctl`
- Basic troubleshooting steps

---

### **Intermediate Topics**

##### 13. Advanced Shell Scripting

- Functions, error handling, and debugging scripts
- Automating repetitive tasks

##### 14. Job Scheduling and Automation

- Using `cron`, `at`, and `systemd` timers
- Introduction to configuration management tools (Puppet, Chef, and Ansible)

##### 15. System Boot Process and GRUB

- Understanding BIOS/UEFI, bootloaders, and runlevels/targets

##### 16. Process Management and Optimization

- Managing priorities (`nice`, `renice`)
- Resource limits (`ulimit`)
- Background/foreground jobs

##### 17. Advanced File System Management

- Working with LVM (Logical Volume Manager)
- RAID configuration and management
- Filesystem tuning and maintenance (`tune2fs`, `fsck`)

##### 18. Backup and Restore Strategies

- Tools: `rsync`, `tar`, `dd`
- Incremental and differential backups
- Disaster recovery planning

---

### **Advanced Topics**

##### 19. Linux Kernel Internals and Optimization

- Kernel modules: loading, unloading, compiling (`lsmod`, `modprobe`, `insmod`, `rmmod`)
- Kernel parameter tuning via `sysctl`
- Kernel updates and live patching

##### 20. Security and Hardening

- User and file security (SELinux, AppArmor)
- SSH hardening and key management
- Firewall configuration (`iptables`, `firewalld`, `ufw`)
- Security audits and patch management

##### 21. Centralized Logging and Monitoring

- Setting up and managing centralized log systems (ELK Stack, Graylog)
- System monitoring tools: `top`, `htop`, `vmstat`, `iostat`, `free`
- Alerting and proactive monitoring

##### 22. Networking: Advanced Configuration and Troubleshooting

- Network performance tuning (TCP stack, `ethtool`)
- VLANs, bonding, and bridging
- DNS, DHCP, and network services management

##### 23. Virtualization and Containers

- KVM, QEMU, and VirtualBox basics
- Docker and Podman for containerization
- Orchestration with Kubernetes

##### 24. High Availability and Load Balancing

- Clustering with Pacemaker, Corosync
- Load balancing with HAProxy, Nginx
- Failover and redundancy strategies

##### 25. Filesystem and Storage: Advanced Topics

- Working with advanced filesystems (XFS, Btrfs, ZFS)
- Storage management for performance and reliability
- Networked storage: NFS, Samba, iSCSI

##### 26. Performance Tuning and Troubleshooting

- Identifying and resolving bottlenecks
- Disk, CPU, and network optimization
- Benchmarking tools and methodologies

##### 27. Automation and Infrastructure as Code

- Scripting for automation (Bash, Python)
- Configuration management (Ansible, Puppet, Chef)
- Version control for system configs (Git)

##### 28. Cloud Integration and Hybrid Environments

- Using Linux in AWS, Azure, GCP
- Automation with cloud-native tools (CloudFormation, Terraform)
- Hybrid and multi-cloud strategies

##### 29. Documentation and Best Practices

- System documentation standards
- Change management and record-keeping
- Collaborative tools and wikis

---

### **Specialized and Emerging Topics**

##### 30. Linux for Developers

- Development toolchains, compilers, and debugging
- Building and patching software from source

##### 31. Programming and Visualization Tools

- Using tools like Mermaid for system diagrams
- Integrating Linux with visualization and monitoring dashboards

##### 32. Linux in DevOps and CI/CD

- Integration with CI/CD pipelines
- Automated testing and deployment

##### 33. Incident Response and Forensics

- Collecting and analyzing logs for incidents
- Forensics tools and methodologies

---

### Disclaimer

The perspectives and opinions expressed in this document are entirely my own and do not represent the views or positions of my current or previous employers.

---
>
> - Link to ["The Kubernetes Show"](https://www.youtube.com/@thekubernetesshow) YouTube channel
