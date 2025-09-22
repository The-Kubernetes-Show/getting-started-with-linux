# Linux Networking Basics

---

[YouTube Video](https://youtu.be/XsHUalXj0rw)

---

## What Is Networking in Linux?

Linux networking enables devices to communicate over local and global networks. Understanding networking means learning about interfaces, IP addresses, routing, and the tools used to manage connectivity.Networking is essential for services like web hosting, file sharing, and remote access. Linux provides powerful command-line tools and configuration files to manage network settings.

---

## Key Networking Components

- **Network Interface**: Physical (Ethernet, Wi-Fi) and virtual interfaces (loopback, bridges).
- **IP Address & Subnet**: Unique identifiers for devices on a network; subnet defines address ranges.
- **Gateway**: Routes traffic from local network to external destinations.
- **DNS**: Resolves hostnames to IP addresses.
- **Routing Table**: Determines packet path through interfaces
- [Networking key concepts e.g. IP, Netmask, Broadcast etc.](https://documentation.ubuntu.com/server/explanation/networking/networking-key-concepts/)

---

## Traffic flow in networking

![networking-diagram](https://thermalcircle.de/lib/exe/fetch.php?media=linux:nf-hooks-detail1.jpg) 
Source: [thermalcircle.de - Nftables - Packet flow and Netfilter hooks in detail](https://thermalcircle.de/doku.php?id=blog:linux:nftables_packet_flow_netfilter_hooks_detail)

---

## Core Configuration Files

### For Ubuntu 24.x and Later

Ubuntu now uses **Netplan** to manage network settings:

- **/etc/netplan/*.yaml**: Main location for network configurations (YAML syntax)
- **/etc/resolv.conf**: DNS resolver settings.
- **/etc/hostname**: System hostname.

Official Ubuntu networking documentation:

- [Ubuntu Introduction to networking](https://documentation.ubuntu.com/server/explanation/intro-to/networking/)
- [Configuring networks with Netplan](https://documentation.ubuntu.com/server/explanation/networking/configuring-networks/)
- [Netplan tutorial](https://netplan.readthedocs.io/en/stable/netplan-tutorial/)


### For RHEL 8 and Higher

RHEL 8 uses **NetworkManager** by default:

- **/etc/NetworkManager/system-connections/**: Profiles for each connection (normally .nmconnection files)
- **nmcli**: Command-line tool for managing connections.
- **nmtui**: Text-based user interface for configuring networks.
- **/etc/sysconfig/network-scripts/**: Legacy location for `ifcfg-*` files, but not default since RHEL 8.

Official RHEL networking documentation:

- [Configuring and managing networking](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/index)

***

## Step-By-Step Networking Tasks

### 1. View Available Network Interfaces

```bash
# Ubuntu & RHEL 8+
ip link show
# or
ifconfig -a

# Check IP Addresses
ip addr show
```

### 1a. Check DNS and Routing

```bash
# To see current DNS settings
cat /etc/systemd/resolved.conf
cat /etc/resolv.conf

# Check routing table
ip route show

# Man pages for systemd-resolved
man systemd-resolved
```

### cloud-init Note: If your system uses cloud-init (common in cloud environments), it may override network settings on reboot.

```bash
# Check if cloud-init is installed
dpkg -l | grep cloud-init  # Ubuntu
rpm -qa | grep cloud-init   # RHEL
# View cloud-init status
sudo cloud-init status
```

```bash
# View cloud-init generated SSH keys (if applicable)
cat ~ubuntu/.ssh/authorized_keys
sudo cloud-init query -f "{{ combined_cloud_config.ssh_authorized_keys }}"
```

```bash
# Check if cloud-init is managing network configuration
sudo cloud-init query -a|grep -i net
```

### 1b. Disable cloud-init Network Management (if applicable)

```bash
# To permanently disable cloud-init’s network management on systems that reset configurations at reboot
echo "network: {config: disabled}" | sudo tee /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
```

Documentation: [Ubuntu](https://documentation.ubuntu.com/server/explanation/networking/networking-key-concepts/), [RHEL](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/index)

---

### 2. Configure Network with Netplan (Ubuntu)

- **Netplan File**

```bash
sudo cat /etc/netplan/50-cloud-init.yaml
```

- **DHCP Example (automatic IP):**

```yaml
network:
  version: 2
  ethernets:
    enp3s0:
      dhcp4: true
```

- **Static IP Example:**

```yaml
network:
  version: 2
  ethernets:
    enp3s0:
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
          addresses: [8.8.8.8,8.8.4.4]
```

- **Apply Changes**

```bash
sudo netplan apply
```

- **To Debug apply**

```bash
sudo netplan --debug apply
```

Documentation: [Netplan tutorial](https://netplan.readthedocs.io/en/stable/netplan-tutorial/)

---

### 3. Configure Network with NetworkManager (RHEL 8)

- **List Connections (Runnning commands as "root" user)**

```bash
nmcli connection show
```

- **Set Static IP**

```bash
nmcli connection modify "System eth0" ipv4.addresses "192.168.1.123/24"
nmcli connection modify "System eth0" ipv4.gateway "192.168.1.1"
nmcli connection modify "System eth0" ipv4.dns "8.8.8.8"
nmcli connection modify "System eth0" ipv4.method manual
nmcli connection up "System eth0"
```

- **DHCP Example**

```bash
nmcli connection modify "System eth0" ipv4.method auto
nmcli connection up "System eth0"
```


Documentation: [RHEL NM guide](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/configuring-an-ethernet-connection_configuring-and-managing-networking#configuring-an-ethernet-connection-by-using-nmcli_configuring-an-ethernet-connection)

***

### 4. Basic Network Troubleshooting

- **Test Connectivity**

```bash
ping google.com
traceroute google.com
```

- **View Routing Table**

```bash
ip route
```

- **Check DNS**

```bash
cat /etc/resolv.conf
```

- **Restart Network Services**

```bash
if grep -q 'Ubuntu' /etc/os-release; then
    # Ubuntu
    sudo systemctl restart systemd-resolved
elif grep -qi 'rhel' /etc/os-release; then
    # RHEL
    sudo systemctl restart NetworkManager
fi

```

--

## Homework Assignment

- Configure your system to use a static IP and custom DNS, then revert to DHCP. Document all steps.
- Use `ping`, `ip`, and `nmcli`/`netplan` to troubleshoot and verify connectivity.
- Summarize the role of `/etc/netplan/` and `/etc/NetworkManager/system-connections/` in your own words.
- Find and bookmark the official documentation pages for both Ubuntu and RHEL networking setup.

### Official Documentation Links

- [Ubuntu Networking documentation](https://documentation.ubuntu.com/server/explanation/intro-to/networking/)
- [Netplan Tutorial](https://netplan.readthedocs.io/en/stable/netplan-tutorial/)
- [RHEL 8 Networking](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/index)
- [udev network interface naming](https://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames/) *--OR--* Read Man pages using command `man systemd.net-naming-scheme`

---

## Summary

This chapter covered the basics of Linux networking, including key components, configuration files, and essential commands. You learned how to configure networks using Netplan on Ubuntu and NetworkManager on RHEL, along with troubleshooting techniques. Mastery of these concepts is crucial for effective system administration and network management in Linux environments.