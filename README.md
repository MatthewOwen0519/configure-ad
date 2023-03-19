<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Domain Controler (DC-1) in Azure
- Create Client (Client-1) in Azure
- Ensure Connectivity between client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client to the Domain
- Setup Remote Desktop for non-administrative users on the Client
- Generate additional users through a script with Powershell

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/VypWiOY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/UyI78Ek.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
- Setup Domain Controler in Azure
  - Create a Virtual Machine
    - Create a Resource Group: "AD-lab" 
    - Name: "DC-1"
    - Set to a region: "East US"
    - Set to use an image of Windows Server 2022
    - Minimum of 2 cpu
    - Create a user name: labuser
    - Create a password
    - Validate and create    
      - Take note of the Resouce Group and Virtual Netowrk (Vnet) that get created at this time

 
<p>
<img src="https://i.imgur.com/DCPtIyi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Set Domain Controller's NIC Private IP address to be static
  - Click on the DC-1 VM in the Virtual Machine tab
  - Click on the Networking tab
  - Click on the NIC labled "Network Interface: ******"
  - Click IP Configurations
  - Click Ipconfig1
  - Change the Assignment from Dynamic to Static and save (This maintains one IP address to stay assigned to our Domain Controller)
 
  
</p>
<br />

<p>
<img src="https://i.imgur.com/6qp3XeL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/kklzGBh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
- Create a Client VM (Client-1)
  - Create a Virtual Machine
    - Add to Rescource Group: "Ad-lab"
    - Name: "Client-1"
    - Set to region: Use the same as the previous VM (East US)
    - Set to use an image of Windows 10
    - Minimum of 2 cpu
    - Check the License Agreement box
    - Validate and create
      - Ensure that this VM is in the same Vnet as DC-1
 
</p>
<br />

<p>
<img src="https://i.imgur.com/7kK0GiT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Ensure Connectivity between the client and Domain Controller
  - Login to Client-1 with Remote Desktop and ping DC-1's private IP (It should time out)
 
</p>
<br />
 
<p>
<img src="https://i.imgur.com/P04xqsZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 - Login to the Domain Controller and enable ICMPv4 on the local Windows Firewall
   - Windows Defender Firewall -> Advanced Settings -> Inbound Rules -> Sort by Protocol and locate ICMPv4 -> [X] Core Network Diagnostics - ICMP Echo Request (ICMPv4 -out) There are two of these to enable, one private, one Domain 
 
</p>
<br />

<p>
<img src="https://i.imgur.com/6oiQzR8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p> 
 
 - Check back at Client-1 to see the ping succeed
 
</p>
<br />

<p>
<img src="https://i.imgur.com/f68ofj9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<p>
<img src="https://i.imgur.com/s8vZSI7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
 
- Install Active Directory
 - Login to DC-1 and install Active Directory Domain Services 
   - Add Roles and Features on the Server Manager window -> Follow the default install prompts until "select a server from the server pool" -> [x] Active Directory Domain Services -> Follow all default install prompts, checking required restart boxes until allowed to install -> Install

</p>
<br /> 
 
<p>
<img src="https://i.imgur.com/Ygy2Lx3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p> 
<p>
<img src="https://i.imgur.com/cUZ18G3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

- Post Deployment Configuration
 - Click the flag in the Server Manager -> Promote this server to a Domain Controller -> Add a new forest "mydomain.com" -> next 

</p>
<br />

<p> 
<img src="https://i.imgur.com/N52HG0r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

- Input a password -> next
  - Follow default prompts until able to install -> Install (this may take a few minutes)
- Restart and log back into DC-1 as user: mydomain.com\labuser

</p>
<br />

<p>
<img src="https://i.imgur.com/CyVBlf2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>

- Create an Admin and Normal User Account in Active Directory
  - In Active Directory Users and Computers (ADUC), create an Organizationl Unit (OU) called "_EMPLOYEES"
    - Tools -> Active directory Users and Computers -> Right-click "mydomain.com" -> new -> Organizational Unit (OU) -> _EMPLOYEES -> Ok
  - Create a new OU named "_ADMINS"
    - Right-click "mydomain.com" -> new -> OU -> _ADMINS -> Ok
</p>
<br />
 
<p> 
<img src="https://i.imgur.com/n9Vz1A6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 - Create a enw employee named "Jane Doe" (same password) with the username of "jane_admin"
   - Right-click "_EMPLOYEES" -> new -> user -> fill in information -> next -> Input password (uncheck to force a password change at login for lab purposes) -> next -> Finish
 
</p>
<br />
 
<p> 
<img src="https://i.imgur.com/x0xLSzv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

 - Add jane_admin to the "Domain Admins" Security group 
   - Double-click "Jane Doe" -> Member of -> add -> Domain Admin -> Check Name -> Ok -> Apply -> Ok
 - Log out/close the Remote Desktop connection to DC-1 and log back in as "mydomain.com\jane_admin"
     - Use jane_admin as your admin account from now on

</p>
<br />

<p> 
<img src="https://i.imgur.com/x0xLSzv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
- Join Client-1 to your domain (mydomain.com)
  - From the Azure Portal, set Client-1's DNS settings to the DC's private IP address
    - Click the Client-1 VM -> Networking -> DNS -> Custom -> insert private IP of DC-1
  - From the Azure Portal, restart Client-1

</p>
<br />

<p> 
<img src="https://i.imgur.com/x0xLSzv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
- Login to Client-1 (Remote Desktop) as the original local admin (labuser) and joing it to the domain (computer will restart)
  - Settings -> System ->

</p>
<br />

<p> 
<img src="https://i.imgur.com/x0xLSzv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
- Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers inside the "Computers" container on the root of the domain
- Create a enw OU named "_CLIENTS" and drag Client-1 into it

</p>
<br />

<p> 
<img src="https://i.imgur.com/x0xLSzv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
- Setup Remote Desktop for non-administrative users on Client-1
  - Log into Client-1 as mydomain.com\jane_admin and open system properties
    - Click "Remote Desktop" -> Allow "domain users" acess to remote desktop
  - You can now log into Client-1 as a normal, non-adminstative user (This would normlly be done with a Group Policy to change multiple systems at once)
