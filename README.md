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

<h2>Deployment and Configuration Steps</h2>

<h3> Step 1: Setup Resources in Azure</h3>

Create two virtual machines and one Resource Group

- The first Virtual machine will be the Domain Controller.
    - Name: DC-1
    - Image: Windows Server 2022
    - Take note of the Virtual Network (vNet) that is automatically created.

<p>
<img src="https://i.imgur.com/l8CDCfv.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/zAsvhPX.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/vs1Lgwm.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>


<h3> Step 2: Set DC-1's Virtual Network Interface Card (vNIC) private IP address to be static. <h3>
    - Go to DC-1's network settings
    - Select Network settings.
    - Select the link next to Network Interface
    - Select IP Configurations > ipconfig1
    - Change the assignment from dynamic to static
      - This ensures DC-1's IP address will not change.

<h3>                               </h3>
<p>
<img src="https://i.imgur.com/QTygkJR.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/wyksGyw.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/bovVm7T.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/tEnWT5v.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 3: Creating the second Virtual Machine </h3>
  - The second virtual machine will be the Client
  - Name: Client-1
  - Image: Windows 10 Enterprise
  - Use the same resource group and vNet as DC-1
  - You may need to mess with the zone in order for the vNet to show up.

<h3>                               </h3>

<p>
<img src="https://i.imgur.com/e5nbjP0.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/Sr9qyvq.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 4: Ensure Connectivity between the Client and Domain Controller</h3>

- Login to Client-1 using Microsoft Remote Desktopp
- Search for Command Prompt and open it
- Ping DC-1's private IP Address (for example, 10.1.0.4)
  - The ping request continually times out due to the fire wall settings.
    - To fix this, we need to enable Echo Requests on DC-1's local Windows firewall.

<h3>                            </h3>

<p>
<img src="https://i.imgur.com/VCGTqps.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/OUHZICp.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/EJ2vGYW.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 5: Enabling Echo Request </h3>
  - Login to DC-1 using Microsoft Remote Desktop
  - Start > Windows Administrative Tools > Windows Defender Firewall with Advanced Security > Inbound Rules
  - Sort the list by protocols
  - Look for ICMPv4 protocols > Right Click on echo requests and Enable Rule. This will now allow the clients DNS server to establish connection with the server.
  - Log back into Client-1 and the command line will automatically being pinging DC-1 Successfully.

<h3>                          </h3>

<p>
<img src="https://i.imgur.com/4Q0qECo.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/al9Y1gF.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/qnlO8oV.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>



<p>
<img src="https://i.imgur.com/N7QaTEB.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/J4q7AEn.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/4n21aqs.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h2> Active Directory</h2>

<h3>Step 6: Install Active Directory</h3>

Log back into DC-1

- Open Server Manager
- Select "Add Roles and Features" > Click Next until Server Roles
- At Server Roles, check "Active Directory Domain Services"
- Select Add Features > Now select Next until you reach confirmation.
- Complete the installation

<h3>                             </h3>

<p>
<img src="https://i.imgur.com/g61ZO9z.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/JqvTOHR.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/4fpWRoj.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/thUoNss.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 7: Promoting to Domain Controller </h3>
- At the top right of the Server Manager Dashboard, click on the flag
- Select "Promote this Server to a Domain Controller"

<h3>                                    </h3>

<p>
<img src="https://i.imgur.com/Zjsbdwr.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/GzXfUhp.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 8: Creating Domain</h3>

- Select "Add a New Forest"
  - Root domain name: mydomain.com
- Select Next
- Create a password and uncheck DND delegation
- Select Next and click next until Prerequisites Check
- Select Install to complete the installation
- Dc-1 will automatically restart once it's done
- Give it a few minutes and log back into DC-1 once it's restarted.

<h3>                                               </h3>

<p>
<img src="https://i.imgur.com/VFnRC6D.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/yBGCguK.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/IiuXrjM.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/gsyjfoC.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 9: Create an Admin and Normal User Account in Active Directory</h3>

- On DC-1, open Server Manager
- Click Tools at the top-right of the screen
- Select Active Directory Users and Computers.

<h3>                                                   </h3>

<p>
<img src="https://i.imgur.com/vHa40cM.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/gxch08F.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 10: Organizational Unit</h3>

- Right-click mydomain.com > New > Select Organizational Unit (OU)
- Create two OUs
  - Name the first "_EMPLOYEES"
  - Name the second "_ADMINS"

<h3>                           </h3>

<p>
<img src="https://i.imgur.com/MQDptHT.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/hlRqn9b.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 11: Admins </h3>

- Right-click mydomain.com and click refresh to sort the new organizational units to the top
- Go to the _ADMINS OU
- Right-click the name of the OU > New > User
  - First/Last name: Jane Doe
  - User login name: jane_admin
  - Select Next
  - Create a password
  - Uncheck all boxes
  - Select Next and then select Finish

<h3>                      </h3>

<p>
<img src="https://i.imgur.com/FltuFWO.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/Oe43fZ0.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/LbZWUoH.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 12: Jane Doe</h3>

- Go to the _ADMINS OU
- Right-click Jane Doe > Select Properties
  - Click the tab named "Member of" > select Add
  - Type in domain admins
  - Select "Check Names" > OK
- Log out of DC-1 and log back in as "mydomain.com\jane_admin"

<h3>                                                    </h3>

<p>
<img src="https://i.imgur.com/CCTFLro.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/3xhrXor.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/cPM1G19.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/1TS1eFm.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 13: Join Client-1 to your domain part 1</h3>

- Go back to the Azure portal
- Navigate to the Client-1 Virutal Machine
- On the left-hand side of the screen select Network settings
- Select the link next to the NIC > select DNS Server > Custom
- Type in DC-1's private IP address
- Click Save
- After it is done updating, Restart DC-1 and Client-1

<h3>                                                                                </h3>

<p>
<img src="https://i.imgur.com/TXOo8LF.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/kSzJsYC.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/vWE92p0.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 14: Join Client-1 to your domain part 2</h3>

- Log back into Client-1 using Microsoft Remote Desktop as yourself
- Open the start menu and select settings > click system > About
- On the right-hand side of the screen, select Rename This PC (Advanced) > Change
- Under "Member of" select Domain
- Type "mydomain.com" and select OK
  - Now type in the password you made and your username.
- Restart the Virtual Machine

<h3>                                                            </h3>

<p>
<img src="https://i.imgur.com/ZVOf8ZB.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/7EYKNtt.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/mvNAJT9.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/Iok4Khb.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 15: Setup Remote Desktop for non-administrative users on Client-1</h3>

- Log back into Client-1 once it's done restarting
- login using mydomain.com\jane_admin and the password you made.
- Left-click the Start menu and open settings then select System
- Select Remote Desktop
- Under User Accounts, click "select Users That Can Remotely Access This PC" > select Add
- Type in domain users
- Select "Check Names" > OK > OK

<h3>                                                                </h3>

<p>
<img src="https://i.imgur.com/HnXE8sw.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/ddpsOB3.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/86qDUDC.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3>Step 16: Create as many additional users as you would like and attempt to log into Client-1 with one of the users' profiles part 1</h3>

- Log back into DC-1 as jane_admin
- Search for powershell_ise
- Right-click on Powershell_ise and open it as an administrator
- At the top-left of the screen select New Script and paste the contents of the following script into it.
  - You can find the script here [https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1]

<h3>                                                                                                                            </h3>

<p>
<img src="https://i.imgur.com/p5Q0Lef.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/JQEjb4y.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/pnTkzpk.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h3> Step 17: Create as many additional users as you would like and attempt to log into Client-1 with one of the users' profiles part 2</h3>

- Click the green arrow button near the top-middle of the screen
  - This will run the script
- Once enough users have been created, go back to Active Directory Users and Computers > mydomain.com > _EMPLOYEES. And you will see all the accounts that were created
- You can now log into Client-1 with one of the accounts that were created.
  - Try logging into Client-1 as one of the users, below being an example.
    - Username: mydomain.com\bid.res
    - Password: Password1

<p>
<img src="https://i.imgur.com/yuBOUc9.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/jQCYX3n.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/OIYP5Ym.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h2> Saving funds after demonstration</h2>

<h3>Step 18: Finishing the test</h3>

- Once your done with this test and won't be using it. Do remember to delete the Resource Group and the Virtual Machines like I showed in osTicket Ticket Lifecycle.
        - https://github.com/TylerJH-IT/ticket-lifecycle
