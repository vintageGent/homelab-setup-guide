# Chapter 2: The pfSense Perimeter

> [!IMPORTANT]
> **Subject**: Virtualized Firewall & Network Isolation
> **Objective**: Securing your lab from the internal home network and the public web.

## 🛡️ Introduction

Visibility without control is a vulnerability. In this chapter, we deploy **pfSense** as a virtual machine within Proxmox to act as our "Gatekeeper." It will manage our internal lab networking, provide DHCP, and handle the firewall rules that keep our research isolated.

## 🏗️ Technical Architecture

We will create two virtual bridges in Proxmox:
1. **vmbr0 (WAN)**: Connected to your physical router (your home network).
2. **vmbr1 (LAN/LAB)**: Isolated virtual network with no physical ports.

## 🚀 Deployment Protocol

### 1. Create the VM in Proxmox
- **OS**: FreeBSD (pfSense base).
- **CPU**: 2 Cores.
- **RAM**: 2GB is plenty for a lab.
- **Network**: 
  - Add **Network Device 1**: Model `virtio`, Bridge `vmbr0`.
  - Add **Network Device 2**: Model `virtio`, Bridge `vmbr1`.

### 2. Initial Configuration
1. Boot the pfSense ISO.
2. Select **Assign Interfaces**:
   - `vtnet0` -> WAN
   - `vtnet1` -> LAN
3. Set LAN IP to `10.0.0.1` (This becomes the gateway for all future lab machines).

### 3. The Web Configurator
Access the UI via a temporary VM on `vmbr1` (like a Linux Mint or Ubuntu desktop):
- Navigate to `10.0.0.1`.
- Login: `admin` / `pfsense`.
- Complete the Setup Wizard.

## 🔒 Security Hardening

### Network Zones (VLANs)
For advanced research, we create sub-networks:
- **MANAGEMENT**: For Proxmox and pfSense access.
- **TOOLS**: For ThreatScope and scanning activities.
- **DMZ**: For public-facing servers (like a test instance of Dispatch).

### Firewall Rule #1: Isolation
Ensure that no machine in the **TOOLS** zone can reach your **HOME NETWORK** unless explicitly allowed.

## 🎯 Next Steps
With the perimeter secure, we move to **Chapter 3: Ubuntu Server Core** to host our first services.
