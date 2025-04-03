
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Part 1: VM Deployment
- Part 2: Observe ICMP Traffic
- Part 3: Configure Firewall (Network Security Group)
- Part 4: Explore other traffic (SSH and DHCP) 

<p>
<h1>☁️ Azure VM Deployment Guide – Part 1 </h1>

This guide walks you through creating two Virtual Machines (one Windows and one Linux) in Azure and ensuring they are in the same virtual network and subnet.

---

## 🌐 Step 1: Sign In to Azure

1. Go to the Azure Portal:  
   👉 [https://portal.azure.com/](https://portal.azure.com/)

---
</p>
<br />


<p>
📁 Step 2: Create a Resource Group

1. In the Azure Portal, search for and open **"Resource groups"**.
2. Click **+ Create**.
3. Name the resource group (e.g., `CyberLab-RG`) and select your preferred region.
4. Click **Review + Create**, then **Create**.

<p>
<img src="https://i.imgur.com/aOsVfIR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

---
</p>
<br />

<p>
💻 Step 3: Create a Windows 10 Virtual Machine

1. Go to **Virtual Machines** → Click **+ Create**.
2. Select:
   - **Image**: Windows 10
   - **Resource Group**: Select the one you just created.
   - **VM Name**: Your choice (e.g., `WinVM`)
   - **Authentication**: Password
   - **Username**: e.g., `labuser`
   - **Password**: e.g., `Cyberlab123!`
3. Allow Azure to **create a new Virtual Network (VNet)** and **Subnet** automatically.
4. Click **Review + Create**, then **Create**.

<p>
<img src="https://i.imgur.com/xFyhiwc.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>

---
</p>
<br />


<p>
🐧 Step 4: Create a Linux (Ubuntu) Virtual Machine

1. Go to **Virtual Machines** → Click **+ Create**.
2. Select:
   - **Image**: Ubuntu (e.g., 20.04 LTS)
   - **Resource Group**: Select the same one used above.
   - **VM Name**: e.g., `LinuxVM`
   - **Authentication Type**: Username and Password
   - **Username**: e.g., `labuser`
   - **Password**: e.g., `Cyberlab123!`
3. **Networking Section**:
   - Select the **same Virtual Network and Subnet** that was created with the Windows VM.

4. Click **Review + Create**, then **Create**.

<p>
<img src="https://i.imgur.com/bGXHWHF.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>

---
</p>
<br />


<p>
🔁 Step 5: Verify Network Setup

1. Go to **Virtual Networks** in the Azure Portal.
2. Click on the VNet used during the VM creation.
3. Under **Connected Devices**, confirm both VMs are listed.
4. Check that both are using the **same Subnet**.

<p>
<img src="https://i.imgur.com/Q44l8Zl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

---

</p>
<br />
