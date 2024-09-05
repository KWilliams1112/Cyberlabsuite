<h1>Cyber Lab Suite</h1>

<h2>Description</h2>
Project consists of building a cybersecurity suite of tools for monitoring, testing and exploitation excercises. In building this lab I hope to demonstrate a level of understanding across various tools, systems and concepts being used in cybersecurity today. 
<br />


<h2>Services, Environments & Utlities Used</h2>

- <b>Proxmox</b> 
- <b>PfSense</b>
- <b>Kali Linux</b>
- <b>Wazuh</b>
- <b>Caldera</b>
- <b>The Hive</b>
- <b>Metasploitable 2</b>
- <b>DVWA</b>
- <b>Windows Server 2022</b>
- <b>Windows 10</b>
- <b>Docker</b>

<h2>Lab Suite</h2>

Firstly we are going to think about our network segmentation using VLANs to isolate different types of traffic for security and management purposes. Let's start by outlining the network configuration.

<h2>Network Configuration</h2>

<h3>VLAN 1 - Management and Security Tools</h3>

- <b>Applications: The Hive, Cortex, Nessus, Kali Linux, Caldera, Wazuh</b>

- <b>Purpose: This VLAN is for your primary security tools and management systems. These tools will be used to monitor, scan and manage the other VLANs.</b>

<h3>VLAN 10 - Vulnerable Systems for Testing</h3>

- <b> Applications: Metasploitable, DVWA </b>

- <b>Purpose: This VLAN contains intentionally vulnerable systems used for testing and exploitation excercises. It allows us to safely practice penetration testing and vulnerability assessments without risking our primary network.</b>

<h3>VLAN 20 - Standard Operating Systems</h3>

- <b>Applications: Windows Server, Windows 10</b>

- <b>Purpose: This VLAN hosts standard operating systems which can be used as targets for security testing or for setting up services that other applications depend on.</b>

<h3>VLAN 30 - Web Application Security Testing</h3>

- <b>Applications: Docker, WebGoat, bW App, DVWA</b>

- <b> Purpose: This VLAN is for web application security testing environments. Docker can be used to easily deploy and manage instances of these applications.</b>

<h2>Using pfSense to Create VLANs</h2>

Setting up routing between VLANs with pfSense in your Proxmox homelab involves configuring pfSense as a virtual router/firewall. This setup will allow devices on different VLANs to communicate with each other while maintaining network segmentation and security. Here’s a step-by-step guide:

<h3>Step 1: Install pfSense on Proxmox</h3>

	1.	Download pfSense ISO:
	   •	Go to the pfSense website and download the ISO image.
	2.	Upload ISO to Proxmox:
	   •	In the Proxmox web interface, go to Datacenter -> Storage -> ISO Images -> Upload.
	   •	Upload the pfSense ISO.
	3.	Create a New VM for pfSense:
	   •	Go to Create VM in Proxmox.
	   •	Name the VM (e.g., “pfSense-Router”).
	   •	Choose the pfSense ISO image as the installation media.
	   •	Configure the VM:
	   •	CPU: 2 Cores
	   •	Memory: 2 GB RAM
	   •	Storage: 10 GB disk space
	   •	Network Configuration: Initially, assign a single network interface (e.g., vmbr0 for LAN).
	4.	Install pfSense:
	   •	Start the VM and follow the pfSense installation prompts.
	   •	Set up the pfSense admin password and complete the installation.

Step 2: Configure VLAN Interfaces in pfSense

	1.	Access pfSense Web Interface:
	   •	After installation, access the pfSense web interface using the IP assigned to the pfSense LAN interface (e.g., https://192.168.1.1).
	2.	Initial Setup:
	   •	Complete the initial setup wizard in pfSense.
	   •	Set the LAN interface IP to 192.168.1.1/24 or any appropriate subnet for VLAN 1.
	3.	Add VLANs to pfSense:
	   •	Go to Interfaces -> Assignments -> VLANs.
	   •	Click Add to create VLANs corresponding to your VLAN setup.
	   •	Example:
	   •	VLAN 10: Parent Interface: vmbr0, VLAN Tag: 10
	   •	VLAN 20: Parent Interface: vmbr0, VLAN Tag: 20
	   •	VLAN 30: Parent Interface: vmbr0, VLAN Tag: 30
	   •	Save the VLAN configurations.
	4.	Assign VLANs to Interfaces:
	   •	Go to Interfaces -> Assignments.
	   •	Assign each VLAN to an interface.
	   •	Example:
	   •	VLAN 10: OPT1
	   •	VLAN 20: OPT2
	   •	VLAN 30: OPT3
	   •	Enable these interfaces and assign appropriate IP addresses for each VLAN:
	   •	VLAN 10: 192.168.10.1/24
	   •	VLAN 20: 192.168.20.1/24
	   •	VLAN 30: 192.168.30.1/24

Step 3: Set Up DHCP for Each VLAN (Optional)

<b>Please note that for the VLAN with the Windows machines its best to use Windows Server to perform DHCP</b>

	1.	Enable DHCP Server:
	   •	Go to Services -> DHCP Server.
	   •	Enable the DHCP server for each VLAN interface (OPT1, OPT2, OPT3).
	   •	Configure the DHCP range for each VLAN
	   •	Example:
	   •	VLAN 10: 192.168.10.100 - 192.168.10.200
	   •	VLAN 20: 192.168.20.100 - 192.168.20.200
	   •	VLAN 30: 192.168.30.100 - 192.168.30.200

Step 4: Configure Firewall Rules

	1.	Set Up Allow Rules Between VLANs:
	   •	By default, pfSense blocks all inter-VLAN traffic. You need to create rules to allow communication between VLANs.
	   •	Go to Firewall -> Rules -> Select the VLAN interface (e.g., OPT1 for VLAN 10).
	   •	Add a rule to allow traffic from VLAN 10 to VLAN 20 and VLAN 30:
	   •	Action: Pass
	   •	Protocol: Any
	   •	Source: VLAN 10 subnet (192.168.10.0/24)
	   •	Destination: VLAN 20 subnet (192.168.20.0/24) or VLAN 30 subnet (192.168.30.0/24)
	   •	Repeat this process for other VLANs to allow desired inter-VLAN communication.
	2.	Test Connectivity:
	   •	Once rules are in place, test the connectivity by pinging devices across VLANs.

Step 5: Configure NAT and Internet Access (Optional)

	1.	Set Up WAN Interface:
	   •	If you want internet access for your VLANs, assign another network interface as a WAN connection in pfSense.
	   •	Configure the WAN interface with your public IP or through DHCP if using a home router.
	2.	Configure NAT:
	   •	Go to Firewall -> NAT -> Outbound.
	   •	Select Hybrid Outbound NAT Rule Generation.
	   •	Add NAT rules for each VLAN interface to allow internet access.
	3.	Test Internet Connectivity:
	   •	From a device in any VLAN, test internet connectivity.








<!---
<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

--!>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
