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
<img src="https://i.imgur.com/twAiZM7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/6V91EDm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
<img src="https://i.imgur.com/m3TARHJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/A8zhqv1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/3ipRMs6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/ArYdfzw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/h5mVtf3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- The second virtual machine will be the Client
  - Name: Client-1
  - Image: Windows 10 Enterprise
  - Use the same resource group and vNet as DC-1

<p>
<img src="https://i.imgur.com/Ydcno8b.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/UVokPP4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>


<h3>Step 2: Ensure Connectivity between the Client and Domain Controller</h3>

- Login to Client-1 using Microsoft Remote Desktop
- Search for Command Prompt and open it
- Ping DC-1's private IP Address (for example, 10.1.0.4)
  - The ping request continually times out due to the fire wall settings.
    - To fix this, we need to enable Echo Requests on DC-1's local Windows firewall.

<p>
<img src="https://i.imgur.com/rv1BKh2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/sMQ8HU5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Login to DC-1 using Microsoft Remote Desktop
  - Start > Windows Administrative Tools > Windows Defender Firewall with Advanced Security > Inbound Rules
  - Sort the list by protocols
  - Look for ICMPv4 protocols > Right Click on echo requests and Enable Rule. This will now allow the clients DNS server to establish connection with the server.
  - Log back into Client-1 and the command line will automatically being pinging DC-1 Successfully.

<p>
<img src="https://i.imgur.com/IyMA8Ls.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/O9KEoqZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/ztnNYkl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/KM7vsaX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/JMiunVQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/Q90UWzR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/aXfq0Zd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
<img src="https://i.imgur.com/3srPy5E.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/LtqnvQ0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/MKb3QHy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/gnrBCy3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/egq9UeP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/nISdo8g.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/0pI8bTs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- At the top right of the Server Manager Dashboard, click on the flag
- Select "Promote this Server to a Domain Controller"

<p>
<img src="https://i.imgur.com/68JUH6m.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/QEOMwtL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Select "Add a New Forest"
  - Root domain name: mydomain.com
- Select Next
- Create a password
- Select Next and click next until Prerequisites Check
- Select Install to complete the installation

<p>
<img src="https://i.imgur.com/zKLx4qD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/8dURnxB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/DnqpzxL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Dc-1 will automatically restart
- Log back into DC-1 once it's restarted.

<h3>Step 4: Create an Admin and Normal User Account in Active Directory</h3>

- On DC-1, open Server Manager
- Click Tools at the top-right of the screen
- Select Active Directory Users and Computers.

<p>
<img src="https://i.imgur.com/4RnQiuK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/kT2vcHB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Right-click mydomain.com > New > Select Organizational Unit (OU)
- Create two OUs
  - Name the first "_EMPLOYEES"
  - Name the second "_ADMINS"

<p>
<img src="https://i.imgur.com/oYysYOW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/7z6l9Ck.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
<img src="https://i.imgur.com/zJIgRaE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/NAh1Upy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Go to the _ADMINS OU
- Right-click Jane Doe > Select Properties
  - Click the tab named "Member of" > select Add
  - Type in the names of your domain administrators
  - Select "Check Names" > OK
- Log out of DC-1 and log back in as "mydomain.com\jane_admin"

<p>
<img src="https://i.imgur.com/DXezYYS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/1hLl0MM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/ivQ9pRV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/kH9zaAO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
<img src="https://i.imgur.com/VwjLAOC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/znAym9z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Log back into Client-1 using Microsoft Remote Desktop as the original local admin
- Open the start menu and select settings > click system > About
- On the right-hand side of the screen, select Rename This PC (Advanced) > Change
- Under "Member of" select Domain
- Type "mydomain.com" and select OK
  - Now type in the password you made and your username.
- Restart the Virtual Machine

<p>
<img src="https://i.imgur.com/8awawoq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/QPbSkZN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/i644IXP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/Iz8Tw6G.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/vJTTHNB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/xwP5t4Z.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/sQjx0se.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>


<h3>Step 6: Setup Remote Desktop for non-administrative users on Client-1</h3>

- Log back into Client-1
- login using mydomain.com\jane_admin and the password you made.
- Right-click the Start menu and select System
- Select Remote Desktop
- Under User Accounts, click "select Users That Can Remotely Access This PC" > select Add
- Type in the name of your domain users
- Select "Check Names" > OK > Select Domain Users > OK

<p>
<img src="https://i.imgur.com/8hsHZf5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/rgSmQqN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/tqqPxDb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 7: Create as many additional users as you would like and attempt to log into Client-1 with one of the users' profiles</h3>

- Log back into DC-1 as jane_admin
- Search for powershell_ise
- Right-click on Powershell_ise and open it as an administrator
- At the top-left of the screen select New Script and past the contents of the following script into it.
  - You can find the script here [https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1]

<p>
<img src="https://i.imgur.com/9qH4pq9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/fe59Svw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/2uwc2xu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Click the green arrow button near the top-middle of the screen
  - This will run the script
- Once the users have been created, go back to Active Directory Users and Computers > mydomain.com > _EMPLOYEES. And you will see all the accounts that were created
- You can now log into Client-1 with one of the accounts that were created.
  - Try logging into Client-1 as one of the users
    - Username: mydomain.com\cix.bav
    - Password: Password1

<p>
<img src="https://i.imgur.com/JCm9lrI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/3SRI1Y0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
