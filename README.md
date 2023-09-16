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

Deploying an on-premises Active Directory within Azure Compute can be a complex process, but I'll break it down into simplified steps to give you a high-level overview. Keep in mind that specific details and configurations can vary depending on your organization's requirements. Here are the steps:

- Step 1: Azure Resources Setup:
Set up Azure resources, including a Virtual Network (VNet).

- Step 2: Create Virtual Machines:
Create a Windows Server 2022 VM named "DC-1" for the domain controller.
Create a Windows 10 VM named "Client-1."

-Step 3: Ensure Connectivity:
Verify that DC-1 and Client-1 are in the same VNet.
Establish network connectivity between them.

-Step 4: Install Active Directory:
Install Active Directory Domain Services on DC-1.
Promote DC-1 as a domain controller.

-Step 5: Create User Accounts:
Create administrative and normal user accounts within Active Directory.

-Step 6: Join Client-1 to the Domain:
Configure Client-1 to use DC-1 as its DNS server.
Join Client-1 to the domain.

-Step 7: Setup Remote Desktop Access:
Configure Remote Desktop settings on Client-1.
Allow domain users access to Remote Desktop.

-Step 8: Create Additional Users:
Use PowerShell on DC-1 to create multiple user accounts.
Observe the newly created accounts in Active Directory.
Test logging in with one of the newly created accounts on Client-1.

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
