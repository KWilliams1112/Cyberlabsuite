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

<h2>Lab Suite Walk-Through:</h2>

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

<h3>Let's get our VMs setup</h3>

For this I will be using proxmox as its my hypervisor of choice, however setting up a virtual machine is relatively the same across the board. We are going to look at the minimum requirements to run each service and use that as our baseline to get everything up and running.

Looking at our network configuration we see that we need a few VLANs setup and we want our Kali Linux machine to be within VLAN 1. So those are going to be the first two things we setup.

Within proxmox we’re going to want to get create a local LAN environment for all of this stuff to live. So on your proxmox node you’re going to want to go to System -> Network -> Create -> Linux Bridge. From here we are going to set up our bridge giving it a name (I left mine default) and the IP address range it will have (I’m going with 10.10.1.0/24). Lastly select apply configuration to get all this applied.

<h3>pfSense VM</h3>

Now with this linux bridge network created we are going to create our pfSense virtual machine. Here are the specs below for the machine.

CPU: 2 Cores

Memory: 2 GB RAM

Storage: 10 GB disk space

(Screenshot)

And we are going to add that linux bridge to this machine as well.

(Screenshot)

Now we are going to start the machine and run through the base configuration.

pfSense will ask a few questions.

Should VLANs be setup now? Choose no here.

Enter your WAN Interface: vtnet0

Enter LAN interface: vtnet1

Now we will set the LAN interfaces IPv4 address and manually configure DHCP

This will be 10.10.1.254/24 and then afterwards we will configure DHCP to be between 10.10.1.50-10.10.1.100.

Once this is all done lets get Kali up and running so we can finish pfSense’s configuration.

 

<h3>Kali Linux VM</h3>

We’ll setup the machine and connect it to our previously created linux bridge and then spin up the VM.

Once it’s all running lets check what IP address our Kali machine was given and make sure we can ping the firewall.

If that is all good lets finish configuring pfSense. Lets navigate to the firewall, login using pfSense’s default login and finish setting up our other VLANs.

Once we sign into pfsense we are going to Add VLANs to pfSense:

Go to Interfaces -> Assignments -> VLANs.

Click Add to create VLANs corresponding to your VLAN setup.

Example:

VLAN 10: Parent Interface: vmbr0, VLAN Tag: 10

VLAN 20: Parent Interface: vmbr0, VLAN Tag: 20

VLAN 30: Parent Interface: vmbr0, VLAN Tag: 30

Save the VLAN configurations.

Assign VLANs to Interfaces:

Go to Interfaces -> Assignments.

Assign each VLAN to an interface.

Example:

VLAN 10: OPT1

VLAN 20: OPT2

VLAN 30: OPT3

Enable these interfaces and assign appropriate IP addresses for each VLAN:

VLAN 10: 10.10.10.1/24

VLAN 20: 10.10.20.1/24

VLAN 30: 10.10.30.1/24

Step 3: Set Up DHCP for Each VLAN (Optional)

Enable DHCP Server:

Go to Services -> DHCP Server.

Enable the DHCP server for each VLAN interface (OPT1, OPT2, OPT3).

Configure the DHCP range for each VLAN.

However since we are running Windows Server we can skip VLAN2 as we will use that for DHCP.









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
