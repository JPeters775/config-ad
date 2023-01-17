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

- Create Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in Active Directory
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into Client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>
<h3>Create Resources in Azure</h3>
<p>

1. Create the Domain Controller Virtual Machine (Window Server 2022) and call it "DC-1". Make sure to remember the Resource Group and Virtual Network (Vnet) for the next VM we will create.<br />
2. Set the Domain Controller's NIC Private IP address to Static.<br />
3. Create the Client VM (Windows 10) and name it "Client-1". Use the Resource Group and Vnet from the first step.<br />
4. Ensure both VMs are in the same Vnet before moving forward or this will not work. <br />

</p>
<img src="https://i.imgur.com/nW7uDSD.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />

<h3>Ensure Connectivity between the Client and Domain Controller</h3>
<p>
1. Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)<br />
2. Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall<br />
3. Check back at Client-1 to see the ping succeed<br />
</p>

<img src="https://i.imgur.com/YCsaMmL.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/v8fboCw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/gTDvkt5.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<br />

<h3>Install Active Directory</h3>
<p>
1. Login to DC-1 and install Active Directory Domain Services<br />
2. Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)<br />
3. Restart and then log back into DC-1 as user: mydomain.com\labuser<br />
</p>
<br />

<img src="https://i.imgur.com/WwJOWZm.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/sGFXDov.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/mwNtsvz.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<br />

<h3>Create an Admin and Normal User Account in Active Directory</h3>
<p>
1. In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”<br />
2. Create a new OU named “_ADMINS”<br />
3. Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”<br />
4. Add jane_admin to the “Domain Admins” Security Group<br />
5. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”<br />
6. User jane_admin as your admin account from now on<br />
</p>
<br />


<img src="https://i.imgur.com/a8B52tj.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/1VypD12.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/FkdeJ17.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/7BoFkvF.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<h3>Join Client-1 to your domain</h3>
<p>
1. From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address<br />
2. From the Azure Portal, restart Client-1<br />
3. Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)<br />
4. Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain<br />
5. Create a new OU named “_CLIENTS” and drag Client-1 into there<br />
</p>
<br />

<p>
<img src="https://i.imgur.com/X41fWBI.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/gdPoTkv.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

</p>
<h3>Setup Remote Desktop for non-administrative users on Client-1</h3>
<p>
1. Log into Client-1 as mydomain.com\jane_admin and open system properties<br />
2. Click “Remote Desktop”<br />
3. Allow “domain users” access to remote desktop<br />
4. You can now log into Client-1 as a normal, non-administrative user now<br />

</p>
<br />
<img src="https://i.imgur.com/J2kUHcF.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>


<h3>Create a bunch of additional users and attempt to log into Client-1 with one of the users</h3>
<p>
1. Login to DC-1 as jane_admin<br />
2. Open PowerShell_ise as an administrator<br />
3. Create a new File and paste the contents of this script into it: <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">Link</a><br />
4. Run the script and observe the accounts being created<br />
5. When finished, open ADUC and observe the accounts in the appropriate OU<br />
6. Attempt to log into Client-1 with one of the accounts (take note of the password in the script)<br />
</p>
<br />

<img src="https://i.imgur.com/20i3Squ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/yu24n4D.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<h3>Conclusion</h3>
<p>
Thank you for reading my tutorial. I hope this helps you.
</p>
