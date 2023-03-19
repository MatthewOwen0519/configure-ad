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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Ensure Connectivity between the client and Domain Controller
  - Login to Client-1 with Remote Desktop and ping DC-1's private IP
 
</p>
<br />
 
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 - Login to the Domain Controller and enable ICMPv4 on the local Windows Firewall
 
</p>
<br />
 
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p> 
 
 - Check back at Clien-1 to see the ping succeed
 
</p>
<br />
