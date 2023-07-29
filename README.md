# configure-ad
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell
  

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/ZKQEmrf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I created the Domain Controller VM using Windows Server 2022 and it is named “DC-1”. Then I took note of the Resource Group and Virtual Network (Vnet) that was created. After, I set Domain Controller’s NIC Private IP address to be static. Then, I created the Client VM (Windows 10) and named it “Client-1”. I used the same Resource Group and Vnet that was created in to ensure that both VMs are in the same Vnet. Then I verified by checking the topology with Network Watcher.

</p>
<br />

<p>
<img src="https://i.imgur.com/vTXLeGr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>
<p>
I logged into Client-1 with Remote Desktop and pinged DC-1’s private IP address with ping -t <ip address> (perpetual ping). Then, I logged into the Domain Controller and enable ICMPv4 on the local Windows Firewall to check back at Client-1 to see if the ping succeed.
</p>
<br />

<p>
<img src="https://i.imgur.com/cRkQdrH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/79nh6Yk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I will log in to DC-1 and proceed to install Active Directory Domain Services. Next, I will promote DC-1 as a Domain Controller by setting up a new forest named mydomain.com (I can choose any name, but I'll remember it as mydomain.com). After completing the promotion, I will restart the server. Once it's back up, I'll log back into DC-1 using the user account mydomain.com\labuser.

</p>

<p>
<img src="https://i.imgur.com/HFE5QxA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), I will create an Organizational Unit (OU) called "_EMPLOYEES." Next, I'll proceed to create another new OU named "_ADMINS." After that, it's time to add a new employee named "Jane Doe" with the same password, and I'll set her username as "jane_admin." To grant administrative privileges, I'll add "jane_admin" to the "Domain Admins" Security Group. Once these steps are done, I'll log out or close the Remote Desktop connection to DC-1 and then log back in as "mydomain.com\jane_admin." From now on, I'll use "jane_admin" as my admin account for managing the domain.
</p>
<br />

<p>
<img src="https://i.imgur.com/17u540q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First, I will go to the Azure Portal and set Client-1's DNS settings to the DC's Private IP address. Next, from the Azure Portal, I will restart Client-1. After the restart, I will log in to Client-1 using Remote Desktop as the original local admin, which is "labuser." Then, I'll join Client-1 to the domain, and this action will trigger another restart. Once Client-1 is back up after joining the domain, I'll log in to the Domain Controller using Remote Desktop. I will then verify that Client-1 shows up in Active Directory Users and Computers (ADUC) inside the "Computers" container located at the root of the domain. To organize the Active Directory structure, I'll create a new Organizational Unit (OU) named "_CLIENTS." Then, I'll simply drag and move Client-1 into the newly created "_CLIENTS" OU.

</p>
<br />

<p>
<img src="https://i.imgur.com/yh3ane0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I will log into Client-1 using the "jane_admin" account (mydomain.com\jane_admin) and open the system properties. Next, I will click on "Remote Desktop" to access the Remote Desktop settings. I will allow "domain users" to have access to Remote Desktop. Now, any normal, non-administrative user can log into Client-1 using Remote Desktop. Normally, for managing multiple systems, I would prefer using Group Policy, which allows me to make changes across many systems at once (perhaps in a future lab).
</p>
<br />

<p>
<img src="https://i.imgur.com/gVuC0Ye.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I will log into DC-1 using the "jane_admin" account. Next, I'll open PowerShell_ise as an administrator. Then, I will create a new file and copy the contents of the script from this link: (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into the file. With the script loaded, I'll run it and observe the accounts being created in Active Directory.
Once the script completes, I will open Active Directory Users and Computers (ADUC) to check the appropriate OU and see the newly created accounts. As a final step, I'll try to log into Client-1 using one of the newly created accounts (making sure to note the password used in the script).


</p>
<br /><br />
