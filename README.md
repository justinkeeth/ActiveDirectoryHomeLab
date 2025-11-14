<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
Project consists of setting up Virtual Home Lab on Windows Server 2025 to run Active Directory and then use a Powershell script to create a bulk list of users to assign to the Active Directory. [The script file and user names txt file were uploaded and attached to this repository, you will want to download them onto the virtual server that is created. -> ('1_CREATE_USERS.ps1' - Powershell script and 'names.txt')].
<br />


<h2>Software versions and Environments Used</h2>

- <b>VirtualBox by Oracle (version 7.2.4)</b>
- <b>Windows Server 2025</b>
- <b>Windows 11 (Pro)</b>
- <b>Windows PowerShell</b>


<h2>Network Diagram:</h2>

<div align="center"><p></p><h3>Network Diagram For Project:</h3><br/>
 <img width="1250" height="754" alt="networkdiagramfinal" src="https://github.com/user-attachments/assets/7855aadd-131d-452b-ae74-b1e4972014bb" />

</div>

<h2>Lab walk-through:</h2>

<p>
 You first download and install VirtualBox by Oracle (You will need to download and install the VirtualBox Extension pack as well). Next step is to download an ISO file for the Windows Server 2019, 2022, or 2025 and an ISO file for Windows 10 or Windows 11 for the client VMs. You will need available hardware to allocate memory, hard disk, and processing power from the CPU (2-4 cores for adequate processing speed) for the virtual machines to run on. While setting up the virtual machines, you will need to configure the amount of hardware to allocate, and then select the ISO file for the Windows Server version you have downloaded. Before you finish configuring the virtual machine, you will need setup 2 Network Adapters, the first network adapter will be set to NAT for the virtual machine to have access your home internet connection, and the 2nd Network adapter will be for the internal domain network that is being created for the Active Directory environment. Next, you will launch the virtual machine into the OS installation for the server and complete the installation process for the operating sysem onto the VM. <br />
 After you complete the operating system installation, you will install the VM guest additions image file and then setup the IP addressing for the network adapter. You will rename the adapter that is for your NAT home network connection 'Internet' or a name you will associate with your home intetet to be able to identify which adapter it is. Next, rename the internal network adapter to 'Internal' or 'domain network'. Make sure you identify which adapter is the internal network and which is the NAT adapter by right-clicking and opening up 'Status' and then 'Details...' to determine the correct adapters by their IP address. The internal network that we created is going to have an APIPA IP address which will start with, 169.xxx.xxx.xxx. After you have renamed them appropriately, open the properties of the internal network adapter and then open the Internet Protocol Version 4 (TCP/IPv4) Properties, and input the IP address from the network diagram image below: IP: 172.16.0.1 , subnet mask: 255.255.255.0, and then leave default gateway blank because this domain controller will serve as the default gateway itself. The DNS settings need to be configured to it's own IP because the domain controller will have Active Directory, which will include automatically install DNS. You should use the loopback address as the DNS server: 127.0.0.1. Also, optional, but you can rename the PC under the system settings to something like 'Domain Controller' if you would like.<br />
Next, we will install the Active Directory Domain Services onto the domain controller, and then create the domain, you can use 'mydomain.com' to make it simple. After the vm restarts, you will open the Active Directory Users and Computers. Go to the mydomain.com dropdown, right-click and go to New -> Organizational Unit. This first Organizational Unit will be for admins, you can name it _admins. Next, you will highlight & right-click the '_admins' OU that we created and create a new user for our administrator account. Name it and set a password for this user account. After you create this user, you will have to right-click and open properties, inside that menu, go to 'Member Of' and then click Add.. and under the object name, type Domain Admins, and press Check Names and then OK and then Apply. Now, we can sign out out of domain controller administrator account, and sign into the user account that we added to the Domain Admins group. You will have to type in the username and password that you created for that user. <br />
Next, we will setup the Remote Access Service on the domain controller, that way we will be able to access the internet through the domain controller when we create a Windows 10 or 11 virtual client, but also remain on the private virtual network. 
For settng up the RAS, we will go to 'add roles and features' on the server dashboard. Click next on the first 2 steps and on the server selection make sure the Domain Controller that you made is selected, click next and then select Remote Access under Features. Click next and under Role Services, you will select Routing and then Add Features, and then click next to advance past the remaining steps.

</p>



<p></p><h2>Visuals of VirtualBox Setup:</h2>
<div align="center">
 <p><h3>VirtualBox Download preview:</h3></p>
<img width="1073" height="266" alt="virtboxprev" src="https://github.com/user-attachments/assets/2c8e43c6-2646-454d-8c0a-8b617fb01b2c" />
<br />
 <p><h3>VirtualBox Server Setup:</h3></p>
<img width="1055" height="559" alt="Screenshot 2025-11-14 110447" src="https://github.com/user-attachments/assets/19c01b91-b988-4fd2-9c11-f4dca7075a1d" />


</div>

<div align="center"><p></p><h3>Video demonstration:</h3><br/>
[Youtube video will be embedded after upload is complete]

</div>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
