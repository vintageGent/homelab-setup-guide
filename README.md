# How to Set Up a Virtual Homelab for Beginners

This guide provides a step-by-step walkthrough for setting up a basic virtual homelab on a Linux desktop. By the end of this guide, you will have a fully functional Ubuntu server running in a virtual machine, managed professionally via SSH, and hosting a basic web page.

## Prerequisites
- A computer running a Linux distribution (this guide uses Ubuntu Desktop).
- An internet connection.

## Part 1: Environment Setup

### 1. Install VirtualBox
VirtualBox is the hypervisor we will use to create and manage virtual machines. If you don't have it, you can install it from the Ubuntu Software Center or by running:
```bash
sudo apt install virtualbox
```

### 2. Install Visual Studio Code (VS Code)
VS Code is a modern code editor that will be our primary tool for managing the server. Install it on your main desktop:
```bash
sudo snap install --classic code
```

## Part 2: Creating the Server VM

### 1. Download Ubuntu Server
Download the latest Long-Term Support (LTS) version of Ubuntu Server. Search for "Ubuntu Server LTS" and download the ISO file from the official website. This guide used **Ubuntu Server 24.04 LTS**.

### 2. Create the Virtual Machine in VirtualBox
1.  Open VirtualBox and click **"New"**.
2.  Configure the basic settings:
    *   **Name:** `ubuntu-server-1`
    *   **ISO Image:** Select the Ubuntu Server ISO file you downloaded.
    *   **RAM:** At least **2048 MB** (2 GB).
    *   **CPUs:** **2** cores.
    *   **Disk Size:** **25 GB** is a good starting point.
3.  On the "Hard Disk" screen, ensure **"Dynamically allocated"** is selected and do not check "Pre-allocate Full Size".

### 3. Configure VM Settings
Before the first boot, select the VM and go to **"Settings"**:
1.  **Storage:**
    *   Go to the **"Storage"** tab.
    *   Make sure the Ubuntu Server ISO is attached to the **Optical Drive**. If it says "Empty", click it and use the disc icon to "Choose a disk file" and select your ISO.
    *   Select the virtual hard disk (e.g., `ubuntu-server-1.vdi`). Ensure it's on **SATA Port 0**.
    *   Check the **"Solid-State Drive"** box.
    *   Leave **"Hot-pluggable"** unchecked.
2.  **Network:**
    *   Go to the **"Network"** tab.
    *   Ensure "Adapter 1" is **Enabled** and "Attached to" is set to **NAT**.

## Part 3: Installing Ubuntu Server

1.  **Start the VM.** It will boot into the Ubuntu installer.
2.  Follow the on-screen instructions. The default options are fine for most steps.
3.  **Storage Configuration:** When prompted, select **"Use an entire disk"**. Confirm the selection of the 25GB VirtualBox disk. This is safe and will not affect your host computer's files.
4.  **Profile Setup:** Create your user account. Choose a server name, a username, and a password. **Remember this username and password.**
5.  **SSH Setup:** **This is critical.** When asked, check the box to **"Install OpenSSH server"**.
6.  **Featured Server Snaps:** Do not select any of the optional server snaps (like Docker). It's better to learn to install these manually later. Proceed by selecting "Done".
7.  **Reboot:** Once the installation is complete, select "Reboot Now". You may see a message like "Failed unmounting /cdrom". This is normal. In the VM's window menu, go to **Devices -> Optical Drives** and click **"Remove disk from virtual drive"**, then press Enter in the VM window.

## Part 4: Remote Management with SSH

Your server is now running. The professional way to manage it is via SSH, not the VirtualBox window.

### 1. Set up Port Forwarding for SSH
To connect to the isolated VM, we need to forward a port.
1.  Shut down the VM.
2.  Go to **Settings -> Network -> Advanced -> Port Forwarding**.
3.  Add a new rule:
    *   **Name:** `SSH`
    *   **Protocol:** `TCP`
    *   **Host Port:** `2222`
    *   **Guest IP:** `10.0.2.15` (This is the default IP for the first NAT VM. You can get the exact IP by logging into the VM console and running `ip addr`).
    *   **Guest Port:** `22`
4.  Save the settings and start the VM.

### 2. Connect from VS Code
1.  Open VS Code on your main desktop.
2.  Open the integrated terminal (`Ctrl+` \).
3.  Connect using the following command, replacing `your_username` with the username you created for the server:
    ```bash
    ssh your_username@localhost -p 2222
    ```
4.  The first time, type `yes` to accept the host authenticity. Then enter your server's password.
5.  Your terminal prompt should change to `your_username@ubuntu-server-1:~$`. You are now controlling your server.

## Part 5: Installing a Web Server (Nginx)

Now, let's install something on your server.

### 1. Update Your Server
First, update the package lists and upgrade the software:
```bash
sudo apt update
sudo apt upgrade -y
```

### 2. Install Nginx
Nginx is a high-performance web server.
```bash
sudo apt install nginx
```

### 3. Check Nginx Status
Verify that the web server is running:
```bash
sudo systemctl status nginx
```
Press `q` to exit the status screen.

## Part 6: Accessing and Editing Your Website

### 1. Set up Port Forwarding for HTTP
Just like with SSH, we need to forward the HTTP port (80).
1.  Shut down the VM.
2.  Go back to **Settings -> Network -> Port Forwarding**.
3.  Add a second rule:
    *   **Name:** `HTTP`
    *   **Protocol:** `TCP`
    *   **Host Port:** `8080`
    *   **Guest IP:** `10.0.2.15`
    *   **Guest Port:** `80`
4.  Save and start the VM.

### 2. View Your Website
Open a web browser on your main computer and navigate to:
`http://localhost:8080`
You should see the "Welcome to nginx!" page.

### 3. Edit the Website
1.  Connect to your server via SSH.
2.  Use the `nano` text editor to modify the default page. (You need `sudo` because it's a system file).
    ```bash
    sudo nano /var/www/html/index.nginx-debian.html
    ```
3.  Change the content (e.g., edit the text inside the `<h1>` tag).
4.  To save and exit `nano`: Press `Ctrl+X`, then `Y`, then `Enter`.
5.  Refresh your browser at `http://localhost:8080` to see your changes live.

Congratulations! You have successfully set up and are now managing your own virtual homelab.
