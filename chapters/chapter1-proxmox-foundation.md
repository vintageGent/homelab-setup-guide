# Chapter 1: The Proxmox Foundation

> [!NOTE]
> **Subject**: Bare-Metal Hypervisor Installation
> **Objective**: Transforming standard hardware into a flexible research host.

## Introduction (The Concept)

Before we touch any buttons, let's understand **what** we are building.

Think of your physical computer as an empty piece of **Land**.
Normally, you build one big house on it (Windows or Mac).

**Proxmox** allows us to turn that land into a **Skyscraper**.
*   **The Hardware**: The foundation/land.
*   **Proxmox (The Hypervisor)**: The Building Manager. It divides the tower into separate floors.
*   **Virtual Machines (VMs)**: The individual apartments. You can have Linux in Apartment 101, Windows in 102, and a firewall in 103.

This guide helps you hire the Building Manager (Install Proxmox) so you can start renting out apartments (Creating VMs).

## Hardware Prerequisites

Before infiltration begins, ensure your host machine meets these requirements:
- **CPU**: 64-bit (Intel V or AMD-V enabled in BIOS).
- **RAM**: Minimum 8GB (Research labs require at least 16GB-32GB for multiple VMs).
- **Storage**: SSD for the OS/Boot and separate HDD/SSD for VM storage.
- **Network**: Wired Ethernet connection (WiFi is unstable for hypervisors).

## 🚀 Installation Protocol

### 1. BIOS Preparation
- Enter BIOS (usually F2, F12, or DEL).
- Enable **Virtualization Technology** (Intel V-x or AMD-V).
- Set **Boot Mode** to UEFI (preferred).
- Disable **Secure Boot**.

### 2. Flash the Image
- Download the latest Proxmox VE ISO from [proxmox.com](https://www.proxmox.com/en/downloads).
- Use **BalenaEtcher** or `dd` to flash the ISO to a USB drive.

### 3. Installation Steps
1. Insert USB and boot from it.
2. Select "Install Proxmox VE".
3. **Target Harddisk**: Select your boot drive.
4. **Country/Timezone**: Set accordingly.
5. **Password & Email**: Use a secure password and a valid email for alerts.
6. **Management Network**:
   - **Hostname**: `seeker-host.local` (or your preferred lab name).
   - **IP Address**: Set a static IP (e.g., `192.168.1.100`).
   - **Gateway**: Your router's IP.

---

## Alternative Path: The Virtual Lab (No USB Required)

If you don't have a spare computer or USB, you can use **Oracle VirtualBox**.

Think of this as **building a model skyscraper inside your existing house**.
Instead of taking over the whole computer (Land), you run the lab as a program on your current Windows/Mac computer.

### Setup Protocol
1.  **Download & Install**: [Oracle VirtualBox](https://www.virtualbox.org/).
2.  **Create "The Foundation"**:
    *   Open VirtualBox -> New.
    *   Name: `Proxmox-Lab` (or `Ubuntu-Host` if skipping Proxmox).
    *   Select Type: `Linux`, Version: `Debian (64-bit)`.
    *   **Memory**: Give it as much as you can spare (e.g., 8192 MB = 8GB).
    *   **Hard Disk**: Create a virtual disk (at least 50GB).
3.  **Boot**: Select the ISO file you downloaded (Proxmox or Ubuntu) and click Start.

> [!TIP]
> **VirtualBox Networking**: When we get to networking, use "Bridged Adapter" mode to let your lab talk to your home WiFi easily.

---

## ⚡ Post-Install Optimization

### The "No-Subscription" Fix
By default, Proxmox looks for an enterprise license. We need to point it to the community repository.

1. Open the Shell in the Proxmox Web UI (`https://[YOUR_IP]:8006`).
2. Run:
   ```bash
   nano /etc/apt/sources.list.d/pve-enterprise.list
   ```
3. Comment out the enterprise line (`#`).
4. Add the no-subscription repo:
   ```bash
   echo "deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription" >> /etc/apt/sources.list.d/pve-install-repo.list
   ```
5. Update: `apt update && apt dist-upgrade -y`

## 🎯 Next Steps
With the foundation solid, we move to **Chapter 2: The pfSense Perimeter** to secure our network zones.



