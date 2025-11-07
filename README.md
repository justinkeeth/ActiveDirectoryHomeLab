<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
Project consists of setting up Virtual Home Lab on Windows Server 2019 (or 2022) to run Active Directory and then use a Powershell script to create a bulk list of users to assign to the Active Directory. (I have uploaded the files needed to run the powershell script and a bulk list of names to assign to the users that we create on this lab. The files are attached to this repository, you will want to download onto the virtual server that is created).
<br />


<h2>Software versions and Environments Used</h2>

- <b>VirtualBox by Oracle (version 7.2.4)</b>
- <b>Windows 10/ Windows 11</b>
- <b>Windows Server 2019 or 2022</b>
- <b>PowerShell</b>



<h2>Lab walk-through:</h2>

<p>
 You first download and install VirtualBox by Oracle (You will need to download and install the VirtualBox Extension pack as well). Next step is to download an ISO file for the Windows Server 2019 or 2022 and an ISO file for Windows 10 or Windows 11. You will need available hardware to allocate memory, hard disk, and processing power from the CPU (2-4 cores for adequate processing speed) for the virtual machines to run on. While setting up the virtual machines, you will need to configure the amount of hardware to allocate, and then select the ISO file for the Windows Server version you have downloaded. Before you finish configuring the virtual machine, you will need setup 2 Network Adapters, the first network adapter will be set to NAT for the virtual machine to have access your home internet connection, and the 2nd Network adapter will be for the internal domain network that is being created for the Active Directory environment. Next, you will launch the virtual machine into the OS installation for the server and complete the installation process for the operating sysem onto the VM. <br />
 After you complete the operating system installation, you will install the VM guest additions image file and then setup the IP addressing for the network adapter. You will rename the adapter that is for your NAT home network connection to Internet or something to be able to identify which adapter it is, and rename the internal network adapter to Internal. After that, open the properties of the internal network adapter and then open the Internet Protocol Version 4 (TCP/IPv4) Properties, and input the IP address from the network diagram image below: IP: 172.16.0.1 , subnet mask: 255.255.255.0, and then leave default gateway blank because this domain controller will serve as the default gateway itself. The DNS settings need to be configured to it's own IP because the domain controller will have Active Directory, which will include automatically install DNS. You should use the loopback address as the DNS server: 127.0.0.1.<br />
Next, we will install the Active Directory Domain Services onto the domain controller, and then create the domain, you can use 'mydomain.com' to make it simple. After the vm restarts, you will open the Active Directory Users and Computers. Go to the mydomain.com dropdown, right-click and go to New -> Organizational Unit. This first Organizational Unit will be for admins, you can name it _admins. Next, you will highlight & right-click the '_admins' OU that we created and create a new user for our administrator account. Name it and set a password for this user account. After you create this user, you will have to right-click and open properties, inside that menu, go to 'Member Of' and then click Add.. and under the object name, type Domain Admins, and press Check Names and then OK and then Apply. Now, we can sign out out of domain controller administrator account, and sign into the user account that we added to the Domain Admins group. You will have to type in the username and password that you created for that user. <br />
Next, we will setup the Remote Access Service on the domain controller, that way we will be able to access the internet through the domain controller when we create a Windows 10 or 11 virtual client, but also remain on the private virtual network. 
For settng up the RAS, we will go to 'add roles and features' on the server dashboard. Click next on the first 2 steps and on the server selection make sure the Domain Controller that you made is selected, click next and then select Remote Access under Features. Click next and under Role Services, you will select Routing and then Add Features, and then click next to advance past the remaining steps.

 
</p>



<div align="center"><p></p><h3>Network Diagram For Project:</h3><br/>
<img width="636" height="382" alt="NICdiagram" src="https://github.com/user-attachments/assets/9622b316-d55d-474e-ab86-6459bd02ab8d" /></p>
 <br />
 <p><h3>VirtualBox Download preview:</h3></p>
<img width="1073" height="266" alt="virtboxprev" src="https://github.com/user-attachments/assets/2c8e43c6-2646-454d-8c0a-8b617fb01b2c" />
<br />
 <p><h3>VirtualBox Server Setup:</h3></p>
 <img width="1074" height="569" alt="virtualsetup" src="https://github.com/user-attachments/assets/29abe0ad-67b3-4d1e-b36f-2ced4bbb7f62" />



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
