<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<!-- <h2>Video Demonstration</h2> ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com) -->

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

<p>Deploying an on-premises Active Directory within Azure Compute can be a complex process, but I'll break it down into simplified steps to give you a high-level overview. Keep in mind that specific details and configurations can vary depending on your organization's requirements. Here are the steps:</p>

<ul>
  <li>Step 1: Azure Resources Setup: Create a Windows Server 2022 VM named "DC-1" for the domain controller. Create a Windows 10 VM named "Client-1".</li>
  <li>Step 2: Ensure Connectivity: Verify that DC-1 and Client-1 are in the same VNet. Establish network connectivity between them.</li>
  <li>Step 3: Install Active Directory: Install Active Directory Domain Services on DC-1. Promote DC-1 as a domain controller.</li>
  <li>Step 4: Create User Accounts: Create administrative and normal user accounts within Active Directory.</li>
  <li>Step 5: Join Client-1 to the Domain: Configure Client-1 to use DC-1 as its DNS server. Join Client-1 to the domain.</li>
  <li>Step 6: Setup Remote Desktop Access: Configure Remote Desktop settings on Client-1. Allow domain users access to Remote Desktop.</li>
  <li>Step 7: Create Additional Users: Use PowerShell on DC-1 to create multiple user accounts. Observe the newly created accounts in Active Directory. Test logging in with one of the newly created accounts on Client-1.</li>
</ul> 

<h2>Deployment and Configuration Steps</h2>

<h3>Step 1: Setup Resources in Azure</h3>
  <ol>
    <li>Deploy a Domain Controller VM (Windows Server 2022) named “DC-1”. Take note of the Resource Group and Virtual Network (Vnet) created during this step. We will use them when we create our Client VM later.
</li>
    <li>To configure DC-1's NIC (Network Interface Card) with a static private IP address, access the Network Interface settings for DC-1. Within the Network Interface settings, locate the IP Configurations section, specifically focusing on "ipconfig1." In this context, you will have the ability to precisely define and set the Private IP address as static. </li>
  </br>
    <p>
<img src="https://i.imgur.com/dMIuL2z.png" height="80%" width="80%" alt="Static IP Address"/>
</p>
  </br>
    <li>Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.</li>
    <li>Ensure that both VMs are in the same Vnet.</li>
  </ol>

<h3>Step 2: Ensure Connectivity</h3>
  <ol>
    <li>Access the Client-1 VM through a Remote Desktop connection.</li>
    <li>Verify network connectivity by initiating a ping operation from Client-1 to DC-1's private IP address. To achieve this, open a command-line interface, such as Command Prompt, and execute the command: `ping -t [private IP of DC-1]`. Initially, this command is expected to result in timeouts.</li>
   <br />
<p>
<img src="https://i.imgur.com/JmWVcud.png" height="80%" width="80%" alt="Successful Ping"/>
</p>
    <li>Enable ICMPv4 Echo Request within the Windows Firewall settings on DC-1. This action is essential to facilitate successful pinging between Client-1 and DC-1. Note the change in ping status.</li>
  </ol>

<br />
    <p>
<img src="https://i.imgur.com/hTS1hEg.png" height="80%" width="80%" alt="Enable ICMPv4 Echo Request"/>
</p>

<h3>Step 3: Install Active Directory</h3>
  <ol>
    <li>Open Server Manager. Add the "Active Directory Domain Services" role. (See image below for details.) Click through to proceed with the installation.</li>
    <li>After completing the installation, proceed to promote DC-1 to a domain controller. Look for the caution triangle symbol with an exclamation point ('!'). (See image below for details.) Click on it to initiate the promotion process. Select the option to add a new forest and specify the forest name as 'mydomain.com.' Additionally, configure DNS options and any other necessary settings.</li>
    <li>DC-1 will automatically restart upon completion of the promotion process. After the restart, log in using a domain user account (e.g., mydomain.com\labuser).</li>
  </ol>

<br />
<p>
<img src="https://i.imgur.com/2NNvE4n.png" height="80%" width="80%" alt="promote dc-1 to controller"/>
</p>
<p>
<img src="https://i.imgur.com/mQnagbQ.png" height="80%" width="80%" alt="active directory installation"/>
</p>


<h3>Step 4: Create User Accounts</h3>
<ol>
  <li>Via Remote Desktop on DC-1, open Active Directory Users and Computers (ADUC) to create two Organizational Units (OU) named "_EMPLOYEES" and "_ADMINS". (See image below for details.) These OUs will be used to organize user accounts.</li>
   </br>

<p>
<img src="https://i.imgur.com/tb9DVMM.png" height="80%" width="80%" alt="create new organizational unit"/>
</p>
  <li>Within the '_ADMINS' OU, create a user account named 'Jane Doe' with the username 'jane_admin.' After creating the user account, right-click on it to access the option for adding the user to groups. In this step, grant 'jane_admin' membership in the 'Domain Admins' Security Group. This membership will confer administrative privileges within the Active Directory domain, allowing 'jane_admin' to perform domain-wide administrative tasks.</li>
  <li>To perform administrative tasks, log in as "mydomain.com\jane_admin." This login can be used on both the DC-1 and Client-1 Virtual Machines, granting administrative access to manage the Active Directory environment and perform other administrative duties.</li>
</ol>


<h3>Step 5: Join Client-1 to the Domain</h3>
<ol>
  <li>Access the Azure Portal and navigate to Client-1's network settings. In the DNS configuration, specify DC-1's private IP address as the DNS server. This step ensures that Client-1 can successfully locate and communicate with the domain controller during the domain join process. After making this configuration, restart Client-1 to apply the DNS settings, including flushing the DNS cache.</li>
  <br />
  <p>
    <img src="https://i.imgur.com/TiW87Ly.png" height="80%" width="80%" alt="Set DNS to Custom"/>
  </p>
  <br />
  <li>Log in to Client-1 with administrative privileges. Open System Properties by right-clicking on "This PC" or "My Computer," selecting "Properties," and then clicking "Advanced system settings." In the System Properties window, navigate to the "Computer Name" tab and click the "Change" button. Choose the option to "Join a domain or workgroup" and enter the previously configured domain name (e.g., "mydomain.com"). When prompted, provide administrative credentials with the necessary privileges to join a computer to the domain. You can use the "jane_admin" account or another with similar privileges.</li>
  <br />
  <p>
    <img src="https://i.imgur.com/RkDvbe6.png" height="80%" width="80%" alt="Join Domain"/>
  </p>
  <br />
</ol>

<h3>Step 6: Setup Remote Desktop Access</h3>
<ol>
  <li>To Setup Remote Desktop for non-administrative users on Client-1, log into Client-1 as mydomain.com\jane_admin and open system properties.</li>
  <li>Inside the System Properties window, navigate to the "Remote" tab. Locate and click on "Remote Desktop."</li>
  <li>In the Remote Desktop settings, allow access for "domain users." This step authorizes users within the domain to establish Remote Desktop connections to Client-1.</li>
  </br>
<p>
<img src="https://i.imgur.com/347Gv6k.png" height="80%" width="80%" alt="User Remote Desktop Access"/>
</p>
</br>
  <li>With Remote Desktop access now configured, normal, non-administrative users can log in to Client-1 using Remote Desktop. Simply use their domain credentials to initiate the connection.
</li>
</ol>

<h3>Step 7: Create Additional Users</h3>

<ol>
  <li>Login to DC-1 as `jane_admin`. Open PowerShell_ise as an administrator.</li>
  <li>Create a new file and paste the contents of the script into it. You can find the script [here](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).</li>
    <li>Run the script and observe the accounts being created.</li>
  <br />
  <p>
  <img src="https://i.imgur.com/X70TU6f.png" height="80%" width="80%" alt="Powershell ISE"/>
  </p>
  
<li>When finished, open ADUC and observe the accounts in the appropriate OU. Attempt to log into Client-1 with one of the accounts (take note of the password in the script).</li>
  <br />
  <p>
    <img src="https://i.imgur.com/M4ppnbm.png" height="80%" width="80%" alt="Active Directory Users"/>
  </p>
  
  <li>You can now log into Client-1 with any of the user accounts created!</li>
    <br />
  <p>
    <img src="https://i.imgur.com/ExpPUeJ.png" height="80%" width="80%" alt="Login Active Directory"/>
  </p>
</ol>

<h2>In Conclusion: On-Premises Active Directory Is Your Path to Seamless Connectivity</h2>

<p>Now that you've successfully completed this tutorial, you've empowered any user to access their account from virtually anywhere within the network. Whether it's the comfort of home, a bustling office, remote workstations, or the dynamic atmosphere of a university campus, the flexibility of your Active Directory deployment in the cloud (Azure) ensures seamless access to resources and a secure login experience. Embrace the possibilities and enjoy the convenience of a well-connected digital ecosystem!</p>
