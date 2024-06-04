<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
I created an Active Directory home lab using Windows Server 2019 to create a domain controller, and then I used a PowerShell script to add 1000 users to the domain. I then created a client with Windows 10 and joined it to the Active Directory domain.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle VirtualBox 7.0</b>

<h2>Environments Used </h2>

- <b>Windows Server 2019</b>
- <b>Windows 10 Pro</b>

<h2>Create Domain Controller VM walk-through:</h2>

<p align="center">
Name Domain Controller and Select Windows Version: <br/>
<img src="https://i.imgur.com/X01tWUV.jpeg" height="90%" width="90%" alt="DC VM Name and Version"/>
<br />
<br />
</p>
<p>
  I used Oracle VirtualBox to create a new virtual machine (VM) that would be the domain controller (DC). I named the virtual machine "DC" and then set the operating system version as "Other Windows (64-bit) since the domain controller used Windows Server 2019.
</p>
<p align="center">
Allocate DC VM Hardware Resources:  <br/>
<img src="https://i.imgur.com/TeQur8M.jpeg" height="90%" width="90%" alt="DC VM Hardware"/>
<br />
<br />
</p>
<p>
  On the next menu screen I allocated hardware resources for the DC VM to use. 
</p>
<p align="center">
Load Windows Server 2019 ISO: <br/>
<img src="https://i.imgur.com/PukJZGc.jpeg" height="90%" width="90%" alt="DC VM Created"/>
<br />
<br />
<img src="https://i.imgur.com/UfabpJe.jpeg" height="90%" width="90%" alt="Load Server 2019 ISO"/>
<br />
<br />
</p>
<p>
  After creating the DC VM, I launched the DC VM and loaded the Windows Server 2019 ISO file to install the operating system. 
</p>


<h2>Configure Domain Controller VM Network Adapters walk-through:</h2>

<p>
  I configured the DC VM to use two network adapters, one for external Internet connections and another for internal client connections. The purpose of this is so that rather than internal clients having a direct Internet connection, the DC will filter network traffic for internal clients that is sent to or received from external networks.
</p>
<p align="center">
Launch Ethernet Settings:  <br/>
<img src="https://i.imgur.com/jDIrKeR.jpeg" height="90%" width="90%" alt="Ethernet Settings"/>
<br />
<br />
</p>
<p>
  In the Ethernet settings menu, I selected "Change adapter options" to configure the network adapaters. 
</p>
<p align="center">
Rename Network Adapters:  <br/>
<img src="https://i.imgur.com/3eWA6Yc.jpeg" height="90%" width="90%" alt="Rename Netowrk Adapters"/>
<br />
<br />
</p>
<p>
  The "Network Connections" menu contained the two network adapters for the DC, but they were labeled "Ethernet" and "Ethernet2." To identify which adapter was external-facing and which adapter was internal-facing, I viewed the IP address assigned to each adapter. One adapter was assigned a public IP address while the other was assigned an APIPA IP address. The Ethernet adapter with the public IP address received its IP address from my home router's DHCP server, so I knew this adapter was being used for external connections. The Ethernet adapter with the APIPA IP address was unable to contact the DHCP server of my home router because it was configured for internal connections only. I renamed each Ethernet adapter based on the type of connection they were configured for. 
</p>
<p align="center">
Assign IP Address to Internal Ethernet Adapter:  <br/>
<img src="https://i.imgur.com/HQac0Qs.jpeg" height="90%" width="90%" alt="Internal IP Address"/>
<br />
<br />
</p>
<p>
  Once the Ethernet adapters were correctly identified and labeled, I configured the IPv4 properties of the internal adapter to correct the APIPA issue. I assigned a static IP address of 172.16.0.1 with a subnet mask of 255.255.255.0. I used the loopback address of the internal adapter for its DNS server settings since Active Dirctory will automatically install DNS and use the DC itself for DNS. 
</p>


<h2>Active Directory Domain Services walk-through:</h2>

<p>
  Once the network adapters were properly configured, I installed Active Directory Domain Services to the DC. 
</p>
<p align="center">
Select "Add roles and features":  <br/>
<img src="https://i.imgur.com/DtltQdN.jpeg" height="90%" width="90%" alt="Add roles and features"/>
<br />
<br />
Install Active Directory Domain Services:  <br/>
<img src="https://i.imgur.com/bS3XEjq.jpeg" height="90%" width="90%" alt="Install Active Directory Domain Services"/>
<br />
<br />
Active Directory Domain Services Installation Complete:  <br/>
<img src="https://i.imgur.com/y305EOu.jpeg" height="90%" width="90%" alt="Active Directory Domain Services Install Complete"/>
<br />
<br />
</p>
<p align="center">
Promote Server to Domain Controller: <br/>
<img src="https://i.imgur.com/7hOrWMA.jpeg" height="90%" width="90%" alt="Promote Server to DC"/>
<br />
<br />
<p>
  After installing Active Directory Domain Services, I then promoted the server to a Domain Controller. This was done by clicking the flag next to the yellow triangle in the top menu, and then selecting "Promote this server to a domain controller."
</p>
<p align="center">
Add a New Forest: <br/>
<img src="https://i.imgur.com/R6ui6hH.jpeg" height="90%" width="90%" alt="Add New Forest"/>
<br />
<br />
</p>
<p>
  In the menu that appeared, I selected "Add a new forest" and then assiged "mydomain.com" as the root domain name. I then proceeded through the following menus and then installed the forest to the server.
</p>





<p align="center">
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

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
