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
