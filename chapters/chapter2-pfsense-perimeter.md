# Chapter 2: The pfSense Perimeter

> [!IMPORTANT]
> **Subject**: Virtualized Firewall & Network Isolation
> **Objective**: Securing your lab from the internal home network and the public web.

## Introduction

##  Introduction (The Gatekeeper)

Imagine your HomeLab is a **private research facility**.
You don't want just anyone walking in from the street (the Internet) or even from your living room (your Home WiFi).

**pfSense** is the **Armed Guard** at the front gate.
*   **The Guard**: Controls who comes in and who goes out.
*   **The Fence (Firewall)**: A wall that blocks everything by default.
*   **The Checkpoint (WAN)**: The only door to the outside world.

In this chapter, we hire the guard (Install pfSense) to protect our secrets.

## Technical Architecture

We will create two virtual bridges in Proxmox:
1. **vmbr0 (WAN)**: Connected to your physical router (your home network).
2. **vmbr1 (LAN/LAB)**: Isolated virtual network with no physical ports.

##  Deployment Protocol

### 1. Create the VM in Proxmox
- **OS**: FreeBSD (pfSense base).
- **CPU**: 2 Cores.
- **RAM**: 2GB is plenty for a lab.
### 1. Create the VM
- **OS**: FreeBSD (pfSense base).
- **CPU**: 2 Cores.
- **RAM**: 2GB is plenty.
- **Network Setup (Crucial!)**:
  - **For Proxmox Users**:
    - Device 1 (WAN): Bridge `vmbr0` (Connects to Internet).
    - Device 2 (LAN): Bridge `vmbr1` (The Private Zone).
  - **For VirtualBox Users**:
    - Adapter 1 (WAN): Set to **Bridged Adapter** (Select your WiFi card).
    - Adapter 2 (LAN): Set to **Internal Network** (Name it `intnet` or `lab-network`).

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

##  Security Hardening

### Network Zones (VLANs)
For advanced research, we create sub-networks:
- **MANAGEMENT**: For Proxmox and pfSense access.
- **TOOLS**: For ThreatScope and scanning activities.
- **DMZ**: For public-facing servers (like a test instance of Dispatch).

### Firewall Rule #1: Isolation
Ensure that no machine in the **TOOLS** zone can reach your **HOME NETWORK** unless explicitly allowed.

## Next Steps
With the perimeter secure, we move to **Chapter 3: Ubuntu Server Core** to host our first services.



  
