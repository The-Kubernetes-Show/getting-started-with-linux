# Software Package Management in Linux

A practical guide to **Linux package management** with hands-on examples for both **Ubuntu (APT)** and **Red Hat (DNF/YUM, RPM)** systems. All commands are sourced from the official Ubuntu and Red Hat documentation.

---

[YouTube Video](TBD)

---

## Introduction

Managing software packages is a fundamental skill in Linux system administration. This guide covers the most common package management tools used in Linux distributions, focusing on Ubuntu and Red Hat-based systems. You'll learn how to install, update, and remove software packages, as well as how to search for packages and manage dependencies using both high-level package managers (APT, DNF/YUM) and low-level tools (RPM).

## Ubuntu (APT) Package Management

**APT** is the recommended tool for managing packages on Ubuntu. It handles installing, updating, removing, and querying software packages.

### Common APT Commands

**Update the package index:**

```bash
sudo apt update
```

This fetches the latest package lists from repositories, ensuring you install the newest versions.

**Install a package (e.g., nmap):**

```bash
sudo apt install nmap
```

**Remove a package:**

```bash
sudo apt remove nmap
```

Add `--purge` to also remove configuration files:

```bash
sudo apt remove --purge nmap
```

**Upgrade all installed packages:**

```bash
sudo apt upgrade
```

**Search for a package:**

```bash
apt search <package_name>
```

**List all installed packages:**

```bash
apt list --installed
```

**Show details about a package:**

```bash
apt show <package_name>
```

**List files installed by a package:**

```bash
dpkg -L <package_name>
```

**Find which package installed a file:**

```bash
dpkg -S /path/to/file
```

Official documentation:

- [Install and manage packages - Ubuntu Server documentation](https://documentation.ubuntu.com/server/how-to/software/package-management/)
- [Apt User's guide](https://www.debian.org/doc/manuals/apt-guide/index.en.html)


## Red Hat (DNF/YUM, RPM) Package Management

**DNF** (or the older **YUM**) is the standard package manager for Red Hat-based systems. **RPM** is used for lower-level package management.

### Common DNF/YUM Commands

**Update package lists:**

```bash
sudo dnf check-update
```

**Install a package (e.g., curl):**

```bash
sudo dnf install curl
```

**Remove a package:**

```bash
sudo dnf remove curl
```

**Upgrade all packages:**

```bash
sudo dnf upgrade
```

**Search for a package:**

```bash
dnf search <package_name>
```

**List all installed packages:**

```bash
dnf list installed
```

**Show package information:**

```bash
dnf info <package_name>
```

**View transaction history:**

```bash
dnf history
```

Undo a transaction:

```bash
sudo dnf history undo <id>
```

Official documentation:

- [Linux package management with YUM and RPM - Red Hat](https://www.redhat.com/en/blog/how-manage-packages)
- [Chapter 12. Package Management with RPM | Deployment Guide](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/5/html/deployment_guide/ch-rpm)


### RPM Commands (Low-level)

**Install an RPM package:**

```bash
sudo rpm -ivh package.rpm
```

**Remove an RPM package:**

```bash
sudo rpm -e package_name
```

**Query installed packages:**

```bash
rpm -qa
```

**Get info about a package:**

```bash
rpm -qi package_name
```


## Hands-on Examples

### Ubuntu

1. **Install multiple packages:**

```bash
sudo apt install git vim htop
```

2. **Simulate an installation (see what would happen):**

```bash
apt -s install nginx
```

3. **Find which package provides a file:**

```bash
dpkg -S /usr/bin/python3
```


### Red Hat

1. **Install multiple packages:**

```bash
sudo dnf install git vim htop
```

2. **List all files installed by a package:**

```bash
rpm -ql vim
```

3. **Undo the last transaction:**

```bash
sudo dnf history undo last
```


## Homework Topics

Encourage your readers to try these tasks on their own systems:

- Add a new repository and install a package from it (APT or DNF).
- Use `apt` or `dnf` to search for and install a package you haven’t used before.
- Remove a package and its configuration files.
- List all installed packages and filter for a specific keyword.
- Use `dpkg` (Ubuntu) or `rpm` (Red Hat) to find out which package installed a particular file.
- Review the transaction history (`dnf history`) and undo a transaction.
- Explore package details and dependencies using `apt show` or `dnf info`.

These exercises will give users practical experience with Linux package management using both Ubuntu and Red Hat tools.

## Conclusion

Package management is a crucial skill for any Linux user or administrator. Mastering tools like APT and DNF/YUM will help you efficiently manage software on your systems, ensuring they are secure, up-to-date, and tailored to your needs.

## Additional Resources

- [Ubuntu Package Management - Official Documentation](https://help.ubuntu.com/lts/serverguide/apt.html)
- [Red Hat - Linux package management with YUM and RPM](https://www.redhat.com/en/blog/how-manage-packages)
- [Using the DNF software package manager](https://docs.fedoraproject.org/en-US/quick-docs/dnf/)
