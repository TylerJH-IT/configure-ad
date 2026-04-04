# configure-ad

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

<h2>Deployment and Configuration Steps</h2>

<h3> Step 1: Setup Resources in Azure</h3>

Create two virtual machines

- The first Virtual machine will be the Domain Controller.
    - Name: DC-1
    - Image: Windows Server 2022
    - Take note of the Virtual Network (vNet) that is automatically created.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Set DC-1's Virtual Network Interface Card (vNIC) private IP address to be static.
    - Go to DC-1's network settings
    - Select Network settings.
    - Select the link next to Network Interface
    - Select IP Configurations > ipconfig1
    - Change the assignment from dynamic to static
      - This ensures DC-1's IP address will not change.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- The second virtual machine will be the Client
  - Name: Client-1
  - Image: Windows 10 Enterprise
  - Use the same resource group and vNet as DC-1

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>


<h3>Step 2: Ensure Connectivity between the Client and Domain Controller</h3>

- Login to Client-1 using Microsoft Remote Desktop
- Search for Command Prompt and open it
- Ping DC-1's private IP Address (for example, 10.1.0.4)
  - The ping request continually times out due to the fire wall settings.
    - To fix this, we need to enable Echo Requests on DC-1's local Windows firewall.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Login to DC-1 using Microsoft Remote Desktop
  - Start > Windows Administrative Tools > Windows Defender Firewall with Advanced Security > Inbound Rules
  - Sort the list by protocols
  - Look for ICMPv4 protocols > Right Click on echo requests and Enable Rule. This will now allow the clients DNS server to establish connection with the server.
  - Log back into Client-1 and the command line will automatically being pinging DC-1 Successfully.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 3: Install Active Directory</h3>

Log back into DC-1

- Open Server Manager
- Select "Add Roles and Features" > Click Next until Server Roles
- At Server Roles, check "Active Directory Domain Services"
- Select Add Features > Now select Next until you reach confirmation.
- Complete the installation

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- At the top right of the Server Manager Dashboard, click on the flag
- Select "Promote this Server to a Domain Controller"

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Select "Add a New Forest"
  - Root domain name: mydomain.com
- Select Next
- Create a password
- Select Next and click next until Prerequisites Check
- Select Install to complete the installation

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Dc-1 will automatically restart
- Log back into DC-1 once it's restarted.

<h3>Step 4: Create an Admin and Normal User Account in Active Directory</h3>

- On DC-1, open Server Manager
- Click Tools at the top-right of the screen
- Select Active Directory Users and Computers.

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Right-click mydomain.com > New > Select Organizational Unit (OU)
- Create two OUs
  - Name the first "_EMPLOYEES"
  - Name the second "_ADMINS"

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Right-click mydomain.com and click refresh to sort the new organizational units to the top
- Go to the _ADMINS OU
- Right-click the name of the OU > New > User
  - First/Last name: Jane Doe
  - User login name: jane_admin
  - Select Next
  - Create a password
  - Uncheck all boxes
  - Select Next and then select Finish

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Go to the _ADMINS OU
- Right-click Jane Doe > Select Properties
  - Click the tab named "Member of" > select Add
  - Type in the names of your domain administrators
  - Select "Check Names" > OK > Apply
- Log out of DC-1 and log back in as "mydomain.com\jane_admin"

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 5: Join Client-1 to your domain</h3>

- Go back to the Azure portal
- Navigate to the Client-1 Virutal Machine
- On the left-hand side of the screen select Network settings
- Select the link next to the NIC > select DNS Server > Custom
- Type in DC-1's private IP address
- Click Save
- After it is done updating, Restart DC-1 and Client-1

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Log back into Client-1 using Microsoft Remote Desktop as the original local admin
- Right-Click the Start menu and select System
- On the right-hand side of the screen, select Rename This PC (Advanced) > Change
- Under "Member of" select Domain
- Type "mydomain.com" and select OK
  - Username: mydomain.com\jane_admin
  - Type in the password and press OK
- Restart the Virtual Machine

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 7: Setup Remote Desktop for non-administrative users on Client-1</h3>

- Log back into Client-1
- login using mydomain.com\jane_admin and the password you made.
- Right-click the Start menu and select System
- On the right-hand side of the screen, select Remote Desktop
- Under User Accounts, click "select Users That Can Remotely Access This PC" > select Add
- Type in the name of your domain users
- Select "Check Names" > OK > OK

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 7: Create as many additional users as you would like and attempt to log into Client-1 with one of the users' profiles</h3>

- Log back into DC-1 as jane_admin
- Search for powershell_ise
- Right-click on Powershell_ise and open it as an administrator
- At the top-left of the screen select New Script and past the contents of the following script into it.
  - You can find the script here [https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1]

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Click the green arrow button near the top-middle of the screen
  - This will run the script
- Once the users have been created, go back to Active Directory Users and Computers > mydomain.com > _EMPLOYEES. And you will see all the accounts that were created
- You can now log into Client-1 with one of the accounts that were created.
  - Try logging into Client-1 as one of the users
    - Username: mydomain.com\base.milu
    - Password: Password1

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
