
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

<p>
<h1>🔍 Part 2: Observing ICMP Traffic </h1>

This guide helps you use Wireshark on a Windows 10 VM in Azure to monitor ICMP traffic when pinging another VM and a public website.

---
</p>
<br />


<p>
💻 Step 1: Connect to Your Windows 10 VM

- **If you're on a Mac**:
  1. Install [Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12)
  2. Open the app and connect using:
     - **IP address of your Windows VM**
     - **Username**: e.g., `labuser`
     - **Password**: e.g., `Cyberlab123!`

<p> <img src="https://i.imgur.com/BSKlhvT.png" height="40%" width="40%" alt="Disk Sanitization Steps"/> </p>

---
</p>
<br />



<p>
🧪 Step 2: Install and Launch Wireshark

1. On the Windows VM, open a browser.
2. Download and install [Wireshark](https://www.wireshark.org/).
3. Open Wireshark and **start capturing packets** on the active Ethernet interface.

<p>
<img src="https://i.imgur.com/pPtQEb5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

---
</p>
<br />


<p>
📡 Step 3: Filter for ICMP Traffic

1. In the Wireshark filter bar, type:

`icmp`

2. Press Enter. This will show only ICMP packets (used in ping requests/replies).

<p>
<img src="https://i.imgur.com/0rGQMZB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

---

</p>
<br />


<p>
📥 Step 4: Ping Ubuntu VM from Windows VM

1. In the Azure Portal, go to your **Ubuntu VM (LinuxVM)**.
2. Copy its **private IP address**.
3. On the Windows VM:
   - Open **Command Prompt** or **PowerShell**
   - Type:
     ```bash
     ping <Ubuntu-Private-IP>
     ```

4. Go back to Wireshark and **observe the ICMP echo requests and replies**.

<p>
<img src="https://i.imgur.com/g9b14YS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

---
</p>
<br />


<p>
🌐 Step 5: Ping a Public Website

1. On the Windows VM, open Command Prompt or PowerShell.
2. Type:

```bash
ping www.google.com
```
Watch Wireshark and observe ICMP traffic and DNS resolution if visible.

<p>
<img src="https://i.imgur.com/sTB4b4O.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

---
</p>
<br />


<p>
<h1>🔐 Part 3: Configuring a Firewall (Network Security Group) </h1>

This part of the lab walks you through controlling ICMP (ping) traffic using Azure Network Security Group (NSG) rules and observing the effect in Wireshark.

---
</p>
<br />


<p>
📡 Step 1: Start a Continuous Ping

1. On your **Windows 10 VM**, open **Command Prompt** or **PowerShell**.
2. Start a **non-stop ping** to the Ubuntu VM's private IP:
   ```bash
   ping <Ubuntu-Private-IP> -t
   ```
3. Keep this window open and running.

<p>
<img src="https://i.imgur.com/zjwMVgk.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>

---
</p>
<br />



<p>
🌐 Step 2: Block ICMP Traffic Using NSG

1. In the **Azure Portal**, go to:
   - **Virtual Machines > LinuxVM > Networking**
2. Click on the **Network Security Group** attached to the network interface.
3. Under **Inbound Rules**, click **Add Rule** or **Edit existing ICMP rule**.
4. Create a **Deny rule**:
   - **Protocol**: ICMP
   - **Source**: Any
   - **Destination**: Any
   - **Action**: Deny
   - **Priority**: Lower than existing Allow rules (e.g., 100)

<p>
<img src="https://i.imgur.com/BhGzPPf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

---
</p>
<br />


<p>
 🛑 Step 3: Observe the Blocked Traffic

1. In the **Windows VM**, switch to **Wireshark**.
2. Watch the ICMP echo requests being sent but **no replies received**.
3. In the **Command Prompt**, you'll see:
   ```
   Request timed out.
   ```

<p>
<img src="https://i.imgur.com/6YscHAw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
---
</p>
<br />



<p>
✅ Step 4: Re-enable ICMP Traffic

1. Return to the **NSG settings** in Azure.
2. Delete the Deny rule or **add a new Allow rule** for ICMP:
   - **Protocol**: ICMP
   - **Source/Destination**: Any
   - **Action**: Allow
   - **Priority**: Lower number than the deny rule (e.g., 100)

<p>
<img src="https://i.imgur.com/YS6YOBs.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
---
</p>
<br />



<p>
🔁 Step 5: Observe Ping Recovery

1. Go back to the **Windows VM**:
   - Ping responses should now start coming in.
2. Wireshark will now show ICMP echo **replies** again.
3. The command line will say:
   ```
   Reply from <Ubuntu-IP>: bytes=32 time=XXms TTL=XX
   ```

<p>
<img src="https://i.imgur.com/lrEjmWt.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
---
</p>
<br />


<p>
 🛑 Step 6: Stop the Ping

1. In the command prompt, press:
   ```
   Ctrl + C
   ```
2. This will stop the continuous ping loop.

---
</p>
<br />
