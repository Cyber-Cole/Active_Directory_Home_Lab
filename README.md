<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
I created an Active Directory home lab using Windows Server 2019 to create a domain controller, and then I used a PowerShell script to add 1000 users to the domain. I then created a client with Windows 10 Pro and joined it to the Active Directory domain.
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
  I used Oracle VirtualBox to create a new virtual machine (VM) that would be the domain controller (DC). I named the virtual machine "DC" and then set the operating system version as "Other Windows (64-bit)" since the domain controller used Windows Server 2019.
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
Launch Active Directory Users and Computers:  <br/>
<img src="https://i.imgur.com/iDmallm.jpeg" height="90%" width="90%" alt="AD Users and Computers"/>
<br />
<br />
</p>
<p>
  Next I launched the Active Directory Users and Computers tool from the Windows start menu to create a new organizational unit (OU).
</p>
<p align="center">
Create New Oranizational Unit: <br/>
<img src="https://i.imgur.com/qMSxdym.jpeg" height="90%" width="90%" alt="Create OU"/>
<br />
<br />
</p>
<p>
  In the Active Directory Users and Computers tool I right-clicked "mydomain.com," selected "New," and then selected "Organizational Unit."
</p>
<p align="center">
Assign Name to OU:  <br/>
<img src="https://i.imgur.com/yQ7yxUJ.jpeg" height="90%" width="90%" alt="Name OU"/>
<br />
<br />
</p>
<p>
  I then named the OU "_ADMINS."
</p>
<p align="center">
Create New User in _ADMINS OU:  <br/>
<img src="https://i.imgur.com/1cC9fSr.jpeg" height="90%" width="90%" alt="New User in _ADMINS OU"/>
<br />
<br />
</p>
<p>
  After I created the _ADMINS OU, I then created a new admin user in the OU. To do this, I right-clicked the _ADMINS OU, selected "New," and then selected "User."
</p>
<p align="center">
New User Information:  <br/>
<img src="https://i.imgur.com/fzZm2Mo.jpeg" height="90%" width="90%" alt="New User Information"/>
<br />
<br />
</p>
<p>
  In the next window I filled in the information to create the new user. 
</p>
<p align="center">
New User Password:  <br/>
<img src="https://i.imgur.com/0keZKWo.jpeg" height="90%" width="90%" alt="New User Password"/>
<br />
<br />
</p>
<p>
  After entering the information for the new user, I created the password for the new user account. 
</p>
<p align="center">
Add New Admin User to Domain Admins: <br/>
<img src="https://i.imgur.com/hGPiXQd.jpeg" height="90%" width="90%" alt="Add New Admin User to _ADMINS OU"/>
<br />
<br />
</p>
<p>
  After adding the user to the _ADMINS OU, I then added the user to Domain Admins to make them an administrator. 
</p>
<p align="center">
Sign in to Admin Account:  <br/>
<img src="https://i.imgur.com/1xU1Fhq.jpeg" height="90%" width="90%" alt="Sign in Admin"/>
<br />
<br />
</p>
<p>
  I signed in to the newly created admin account to verify it worked properly.
</p>



<h2>Routing and Remote Access Service walk-through:</h2>


<p align="center">  
Install Remote Access: <br/>
<img src="https://i.imgur.com/D9rCo0C.jpeg" height="90%" width="90%" alt="Install Remote Access"/>
<br />
<br />
<img src="https://i.imgur.com/ZQshbhS.jpeg" height="90%" width="90%" alt="Install routing"/>
<br />
<br />
</p>
<p>
  After signing in to the newly created admin account, I used the "Add roles and features" option in Server Manager to add Remote Access. This is how internal clients will connect to the internal Ethernet adapter of the DC so that the DC can then allow Internet connections for internal clients. 
</p>
<p align="center">
Configure Routing and Remote Access:  <br/>
<img src="https://i.imgur.com/HnLKlyB.jpeg" height="90%" width="90%" alt="Configure Routing and Remote Access"/>
<br />
<br />
<img src="https://i.imgur.com/g434Qqf.jpeg" height="90%" width="90%" alt="Configure Routing and Remote Access 2"/>
<br />
<br />
<img src="https://i.imgur.com/OjTg7BK.jpeg" height="90%" width="90%" alt="Select NAT"/>
<br />
<br />
<img src="https://i.imgur.com/Vk8Y9Vz.jpeg" height="90%" width="90%" alt="Select External NIC for NAT"/>
<br />
<br />
</p>
<p>
  After installing the Remote Access feature, I then configured Routing and Remote Access for the DC. To do this, I selected "Routing and Remote Access" from the Tools menu in Server Manager. Next I right-clicked the DC and selected "Configure and Enable Routing and Remote Access." I then selected NAT service and assigned it to the external Ethernet adapter. 
</p>


<h2>DHCP Service walk-through:</h2>


<p align="center">
Install DHCP Service:  <br/>
<img src="https://i.imgur.com/GXecHS6.jpeg" height="90%" width="90%" alt="Install DHCP"/>
<br />
<br />
</p>
<p>
  I selected "Add roles and features" to install the DHCP service to the DC.
</p>
<p align="center">
Launch DHCP Tool:  <br/>
<img src="https://i.imgur.com/lBJXVZ0.jpeg" height="90%" width="90%" alt="Launch DHCP"/>
<br />
<br />
</p>
<p>
  Next I began configuring the DHCP service by launching it from the Tools menu in Server Manager.
</p>
<p align="center">
Create and Name New DHCP Scope: <br/>
<img src="https://i.imgur.com/9sK1UGR.jpeg" height="90%" width="90%" alt="Name DHCP Scope"/>
<br />
<br />
</p>
<p>
  I created a new IPv4 scope and named it "172.16.0.100-200."
</p>
<p align="center">
Define DHCP Scope:  <br/>
<img src="https://i.imgur.com/Qu6jSFT.jpeg" height="90%" width="90%" alt="Define DHCP Scope"/>
<br />
<br />
</p>
<p>
  I then assigned the IP address range of 172.16.0.100-200 to the DHCP scope, as well as assigning a 24 bit subnet mask.
</p>
<p align="center">
Add Router for DHCP: <br/>
<img src="https://i.imgur.com/CG18mcO.jpeg" height="90%" width="90%" alt="Add Router for DHCP"/>
<br />
<br />
</p>
<p>
  Next, I assigned the internal Ethernet adapater of the DC as the router (default gateway) for the DHCP service. 
</p>
<p align="center">
Add Domain Name and DNS Servers to DHCP:  <br/>
<img src="https://i.imgur.com/uQLt5CU.jpeg" height="90%" width="90%" alt="Add Domain Name and DNS to DHCP"/>
<br />
<br />
</p>
<p>
  Finally, I assigned the domain name and DNS server to be used by the DHCP service. 
</p>


<h2>Add 1,000 Domain Users walk-through:</h2>


<p align="center">
Text File With Names:  <br/>
<img src="https://i.imgur.com/lPG3QtP.jpeg" height="90%" width="90%" alt="Name File"/>
<br />
<br />
</p>
<p>
  To add 1,000 domain users to my Active Directory environment, I used a text file containing 1,000 randomly generated names. 
</p>
<p align="center">
Run Windows PowerShell ISE as Administrator:  <br/>
<img src="https://i.imgur.com/Xdc1s4P.jpeg" height="90%" width="90%" alt="PowerShell ISE Admin"/>
<br />
<br />
</p>
<p>
  I launched Windows PowerShell ISE as Administrator to use for running my script that would add 1,000 domain users from the name file to my Active Directory environment. 
</p>
<p align="center">
Load Scipt into PowerShell ISE:  <br/>
<img src="https://i.imgur.com/PZTWu5T.jpeg" height="90%" width="90%" alt="Load Script"/>
<br />
<br />
</p>
<p align="center">
Change File Directory: <br/>
<img src="https://i.imgur.com/TWNdt1h.jpeg" height="90%" width="90%" alt="Change File Directory"/>
<br />
<br />
<img src="https://i.imgur.com/JbVZVyV.jpeg" height="90%" width="90%" alt="List Files"/>
<br />
<br />
</p>
<p>
  After loading in the script to be used, I changed the file directly to where the text file containing the 1,000 random names was located. 
</p>
<p align="center">
Execute Script:  <br/>
<img src="https://i.imgur.com/5uBt64z.jpeg" height="90%" width="90%" alt="Execute Script"/>
<br />
<br />
</p>
<p align="center">
Users Added to Active Directory Environment: <br/>
<img src="https://i.imgur.com/uadCLU6.jpeg" height="90%" width="90%" alt="Users Added"/>
<br />
<br />
</p>


<h2>Client VM walk-through:</h2>


<p align="center">
Client VM Created:  <br/>
<img src="https://i.imgur.com/sZaf7Jr.jpeg" height="90%" width="90%" alt="Client VM Created"/>
<br />
<br />
</p>
<p>
  I created another VM that was used as the client PC to connect to the Active Directory domain and allow the previously created users to access the domain. I set the network adapter for this VM to be internal only, so it could not connect to the Internet directly. Instead, the client PC would have to use the DC to connect to the Internet.
</p>
<p align="center">
Test Internet Connectivity for Client:  <br/>
<img src="https://i.imgur.com/WfHE41R.jpeg" height="90%" width="90%" alt="Internet Connectivity Client"/>
<br />
<br />
</p>
<p>
  To ensure Routing and Remote Access was properly working on the DC, I opened a command prompt on the client PC and pinged google.com. I also checked the network configuration of the client PC to ensure the DHCP service of the DC was properly working. 
</p>
<p align="center">
Change Client Name and Join Domain:  <br/>
<img src="https://i.imgur.com/c58SFfG.jpeg" height="90%" width="90%" alt="Change Client Name"/>
<br />
<br />
</p>
<p>
  I changed the name of the client PC so it could easily be identified. I then joined the client PC to the "mydomain.com" domain.
</p>
<p align="center">
Verify Client IP Address Lease:  <br/>
<img src="https://i.imgur.com/XMTkhIb.jpeg" height="90%" width="90%" alt="Client IP Address Lease"/>
<br />
<br />
</p>
<p>
  Using the DC, I verified that the client PC was properly assigned an IP address lease.
</p>
<p align="center">
Sign In as Domain User (dwillmore): <br/>
<img src="https://i.imgur.com/Febi3Hd.jpeg" height="90%" width="90%" alt="dwillmore"/>
<br />
<br />
</p>
<p>
  On the client PC I logged in to one of the user accounts created by the PowerShell script to verify Active Directory Services were properly working. 
</p>
<p align="center">
Verify Client Appears on DC:  <br/>
<img src="https://i.imgur.com/7JvoJEt.jpeg" height="90%" width="90%" alt="Client on DC"/>
<br />
<br />
</p>
<p>
  Finally, I verified the client PC properly appears as a Computer on the DC.
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
