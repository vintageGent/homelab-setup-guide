# Chapter 3: Ubuntu Server Core

> [!NOTE]
> **Subject**: Linux Server Hardening & Service Hosting
> **Objective**: Building the reliable workhorse of the lab.

## Introduction

## Introduction (The Workhorse)

If pfSense is the Gatekeeper, **Ubuntu Server** is the **Employee** who actually does the work.

This is the machine where you will host your tools (like hosting **The Lede** or running **ThreatScope**). 

We use a "Gold Template" approach:
1.  Build one perfect server.
2.  Secure it.
3.  Clone it whenever we need a new worker. No need to reinstall from scratch!

## Deployment Protocol

### 1. VM Creation
- **CPU**: 2 Cores.
- **RAM**: 2GB-4GB.
### 1. VM Creation
- **CPU**: 2 Cores.
- **RAM**: 2GB-4GB.
- **Network**:
  - **Proxmox**: Bridge `vmbr1` (The Protected Zone).
  - **VirtualBox**: Network Adapter 1 -> **Internal Network** (`intnet` or `lab-network`).

### 2. Base Hardening
Once installed, run the Seeker's hardening script (or these manual steps):

1. **Update Everything**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. **Setup non-root user**:
   - Ensure you are not running as `root`. Use `sudo`.
3. **SSH Keys Only**:
   - Disable password login for SSH.
   - Edit `/etc/ssh/sshd_config`:
     ```bash
     PasswordAuthentication no
     PermitRootLogin no
     ```
4. **Install UFW (Uncomplicated Firewall)**:
   ```bash
   sudo ufw allow OpenSSH
   sudo ufw allow 80,443/tcp
   sudo ufw enable
   ```

## Internal Service Stack

For hosting tools like a test version of **The Lede**, we use **Docker** for containerization:

```bash
# Install Docker
sudo apt install docker.io -y
sudo systemctl enable --now docker
sudo usermod -aG docker ${USER}
```

## The Lab is Operational

Congratulations, Seeker. You now have a secure, virtualized environment to build, break, and test your tools.

### What's Next?
- Integrate **ThreatScopeV2** to scan your internal pfSense networks.
- Deploy a private instance of **The Lede** for secure internal communications.
- Document every "Disaster" and "Rebuild" in your field notes.

