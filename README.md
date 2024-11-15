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

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>


PREPARING THE INFRASTRUCTURE FOR ACTIVE DIRECTORY IN AZURE

![Screenshot 2024-11-14 140040](https://github.com/user-attachments/assets/5e6510db-7193-4376-9865-2a4c0a3ba4b4)


![Screenshot 2024-11-14 140656](https://github.com/user-attachments/assets/e8b16ba7-92b6-4904-9bff-9f2f76b8722b)


<p>

![Screenshot 2024-11-14 140825](https://github.com/user-attachments/assets/3291cb55-8dd2-4526-aa3d-973de6202ec8)

![Screenshot 2024-11-14 140958](https://github.com/user-attachments/assets/2efdc526-311b-46d8-a119-56c1745043ad)

</p>
<p>

Setup Domain Controller in Azure
—
Create a Resource Group

Create a Virtual Network and Subnet

Create the Domain Controller VM (Windows Server 2022) named “DC-1”

Username: labuser

Password: Cyberlab123!

After VM is created, set Domain Controller’s NIC Private IP address to be static

Log into the VM and disable the Windows Firewall (for testing connectivity)

</p>
<br />

<p>

![Screenshot 2024-11-14 141412](https://github.com/user-attachments/assets/d7e132c5-16f7-474d-a981-6051261b3c7d)

</p>
<p>

Create the Client VM (Windows 10) named “Client-1”

Username: labuser

Password: Cyberlab123!

Attach it to the same region and Virtual Network as DC-1

After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address

</p>
<br />

RESTART THE CLIENT VM IN AZURE FOR CHANGES TO TAKE PLACE

<p>

![Screenshot 2024-11-14 141753](https://github.com/user-attachments/assets/7826ce5a-af41-4759-a436-a720fba45823)

![Screenshot 2024-11-14 141849](https://github.com/user-attachments/assets/3dd0d0c1-d23e-4ed1-b341-8a15d9749fa8)

</p>
<p>

Login to Client-1

Attempt to ping DC-1’s private IP address

Ensure the ping succeeded

From Client-1, open PowerShell and run ipconfig /all

The output for the DNS settings should show DC-1’s private IP Address

</p>
<br />

SERVER AND CLIENT ARE NOW CONFIGURED AND ACTIVE DIRECTORY CAN NOW BE DEPLOYED

<p>

![Screenshot 2024-11-14 153912](https://github.com/user-attachments/assets/8746c9bd-ea92-4655-bf84-e976d43c7c0d)

![Screenshot 2024-11-14 154103](https://github.com/user-attachments/assets/09928a75-d36b-4da0-bdf8-f0a68cdbdaef)

</p>
<p>
Login to DC-1 and install Active Directory Domain Services

Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)

Restart and then log back into DC-1 as user: mydomain.com\labuser

</p>
<br />

MAKE SURE YOU LOG IN WITH MYDOMAIN.COM\LABUSER

Create a Domain Admin user within the domain



<p>

![Screenshot 2024-11-14 154806](https://github.com/user-attachments/assets/9bc9274b-3c98-473f-b756-673754565bee)

![Screenshot 2024-11-14 154859](https://github.com/user-attachments/assets/85240883-4963-4e9a-84b7-91e05a1f4894)

![Screenshot 2024-11-14 154954](https://github.com/user-attachments/assets/88297056-d8fc-4a29-840d-402f252eb925)

</p>
<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

Create a new OU named “_ADMINS”

Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123!

Add jane_admin to the “Domain Admins” Security Group

Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”

User jane_admin as your admin account from now on

</p>
<br />

<p>

![Screenshot 2024-11-14 155431](https://github.com/user-attachments/assets/ace24272-e14b-45fb-8e43-2b0df8894b33)

![Screenshot 2024-11-14 155612](https://github.com/user-attachments/assets/013de06e-cd8c-4547-8ac3-0254ce9ceb73)


</p>
<p>
Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)

Login to the Domain Controller and verify Client-1 shows up in ADUC

Create a new OU named “_CLIENTS” and drag Client-1 into there

</p>
<br />

<p>

![Screenshot 2024-11-14 161518](https://github.com/user-attachments/assets/9fe2a047-adca-479a-9d24-d541effc033d)


</p>
<p>
Setup Remote Desktop for non-administrative users on Client-1

—

Log into Client-1 as mydomain.com\jane_admin

Open system properties

Click “Remote Desktop”

Allow “domain users” access to remote desktop

You can now log into Client-1 as a normal, non-administrative user now

Normally you’d want to do this with Group Policy that allows you to change MANY systems at once

</p>
<br />

<p>

![Screenshot 2024-11-14 162258](https://github.com/user-attachments/assets/2aa9b321-7678-4281-b3c5-79ba6574322a)

![Screenshot 2024-11-14 162455](https://github.com/user-attachments/assets/f6bf05db-c153-4956-81da-2408753e3469)

</p>
<p>

Create a bunch of additional users and attempt to log into client-1 with one of the users

—

Login to DC-1 as jane_admin

Open PowerShell_ise as an administrator

Create a new File and paste the contents of the [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into it:

Run the script and observe the accounts being created

When finished, open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES)
attempt to log into Client-1 with one of the accounts (take note of the password in the script)

</p>
<br />


DEALING WITH ACCOUNT LOCKOUTS/ ENABLING AND DISABLING ACCOUNTS/ OBSERVING LOGS

Dealing with Account Lockouts

Get logged into dc-1

Pick a random user account you created previously

Attempt to log in with it 10 times with a bad password

Configure Group Policy to Lockout the account after 5 attempts:


[How to Configure Account Lockout Procedure](https://github.com/jhweatherholtz/account-lockout) 



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

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
