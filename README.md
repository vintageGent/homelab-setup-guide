# My Homelab Setup Guide

Hey there, fellow seeker! I'm Mwithiga.

I've always been a tinkerer, driven by a curiosity to understand how the digital world works from the ground up. This curiosity led me down the rabbit hole of self-hosting and building my own homelab. It started with a simple question: "What does it take to build my own private cloud and server infrastructure?"

This repository is the answer to that question. It's my personal, living document detailing the steps, challenges, and configurations I've used to build and maintain my own homelab. I created it to organize my own notes and to share my journey with other enthusiasts who are starting out.

## The Guides

This project is a collection of step-by-step guides for setting up various components of a homelab.

- **Virtualization:** [Proxmox Setup Guide](./Proxmox_Setup.md)
- **Networking:** [Router Setup Guide](./Router_Setup.md)
- _... (and more to come)_

## The Development Journey

The path to a working homelab was filled with learning experiences. Here are some of the key challenges I faced and how I solved them.

### Problem 1: The Foundation - Hardware and Hypervisor Choice

My first hurdle was the paradox of choice. What hardware should I use? An old PC? A Raspberry Pi? A dedicated server? And what operating system should run it all? A standard Linux distro, or a specialized hypervisor like Proxmox or ESXi? The options felt overwhelming.

**Solution:** After a lot of research, I settled on using an old desktop PC to start, as it was the most cost-effective path. I chose Proxmox as the hypervisor because it's open-source, incredibly powerful, and has a fantastic community. This guide starts there, showing how I turned a regular PC into a powerful host for all my virtual machines.

### Problem 2: The Networking Nightmare

Once Proxmox was running, I hit my next wall: networking. How do I make my virtual machines (VMs) accessible on my network? How do I create separate, secure network zones? Understanding concepts like virtual bridges, VLANs, and firewall rules was a steep learning curve.

**Solution:** This challenge forced me to learn the fundamentals of software-defined networking. I decided to document the entire process, from configuring the Proxmox network itself to setting up a dedicated router VM using pfSense. This section of the guide breaks down how to create a "template" for your network that you can reuse for any new service you want to host.

### Problem 3: "I Forgot Everything!"

The real motivation for this project came six months after my initial setup. A power outage corrupted a disk, and I had to rebuild a part of my lab. I was shocked to realize I had forgotten many of the crucial commands and configuration tweaks I had made. My notes were scattered across different text files and were almost useless.

**Solution:** This is when I committed to creating this structured guide on GitHub. It became more than just a set of instructions; it became my personal, centralized knowledge base. By writing each guide as a self-contained chapter, I created a disaster recovery plan that I (or anyone else) could follow from start to finish. It transformed my messy notes into a clear story of my homelab's architecture.