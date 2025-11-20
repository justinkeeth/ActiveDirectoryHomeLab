<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
Project consists of setting up Virtual Home Lab on Windows Server 2025 to run Active Directory and then use a Powershell script to create a bulk list of users to assign to the Active Directory. [The script file and user names txt file were uploaded and attached to this repository, you will want to download them onto the virtual server that is created. -> ('1_CREATE_USERS.ps1' - Powershell script and 'names.txt')].
<br />


<h2>Software versions and Environments Used</h2>

- <b><a href="https://www.virtualbox.org/wiki/Downloads">VirtualBox by Oracle (version 7.2.4)</a></b>
- <b><a href="https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025">Windows Server 2025</a></b>
- <b><a href="https://www.microsoft.com/en-us/software-download/windows11">Windows 11 (Pro)</a></b>
- <b>Windows PowerShell</b>

<div align="center"><p></p><h3>Video demonstration:</h3><br/>
<a href="https://www.youtube.com/watch?v=XroVMhYW4ls" target="_blank" rel="noopener noreferrer"><img width="512" height="288" alt="networkdiagramfinal" src="https://github.com/user-attachments/assets/3bc79949-6396-43aa-88e8-02538bdac1e4" /></a>
 

</div>
<h2>Network Diagram:</h2>

<div align="center"><p></p><h3>Network Diagram For Project:</h3><br/>
 <img width="1250" height="754" alt="networkdiagramfinal" src="https://github.com/user-attachments/assets/7855aadd-131d-452b-ae74-b1e4972014bb" />

</div>

<h2>Lab walk-through:</h2>

<p>
 You first download and install VirtualBox by Oracle (You will need to download and install the VirtualBox Extension pack as well). Next step is to download an ISO file for the Windows Server 2025 and an ISO file for Windows 11 for the client VMs. You will need available hardware to allocate memory, hard disk, and processing power from the CPU (2-4 or more cores for adequate processing speed) for the virtual machines to run on. While setting up the virtual machine server, you will need to configure the amount of hardware to allocate to the server, and then select the ISO file for the Windows Server version you have downloaded. Before you finish configuring the virtual machine, you will need setup 2 Network Adapters, the first network adapter will be set to NAT for the virtual machine to have access your home internet connection, and the 2nd Network adapter will be for the internal domain network that is being created for the Active Directory environment. Next, you will launch the virtual machine into the OS installation for the server and complete the installation process for the operating sysem onto the VM. Note, you will need to select an option with Desktop Experience or there will not be a GUI (Graphical User Interface). You would have to manage the server from the Command Prompt and PowerShell. <br />
 After you complete the operating system installation, you will install the VM guest additions image file and then setup the IP addressing for the network adapter. You will rename the adapter that is for your NAT home network connection 'Internet' or a name you will associate with your home intetet to be able to identify which adapter it is. Next, rename the internal network adapter to 'Internal' or 'domain network'. Make sure you identify which adapter is the internal network and which is the NAT adapter by right-clicking and opening up 'Status' and then 'Details...' to determine the correct adapters by their IP address. The internal network that we created is going to have an APIPA IP address which will start with, 169.xxx.xxx.xxx. After you have renamed them appropriately, open the properties of the internal network adapter and then open the Internet Protocol Version 4 (TCP/IPv4) Properties, and input the IP address from the network diagram image below: IP: 172.16.0.1 , subnet mask: 255.255.255.0, and then leave default gateway blank because this domain controller will serve as the default gateway itself. The DNS settings need to be configured to it's own IP because the domain controller will have Active Directory, which will include automatically install DNS. You should use the loopback address as the DNS server: 127.0.0.1. Also, optional, but you can rename the PC under the system settings to something like 'Domain Controller' if you would like.<br />
Next, we will install the Active Directory Domain Services onto the domain controller, and then create the domain, you can use 'mydomain.com' to make it simple. After the vm restarts, you will open the Active Directory Users and Computers. Go to the mydomain.com dropdown, right-click and go to New -> Organizational Unit. This first Organizational Unit will be for admins, you can name it _admins. Next, you will highlight & right-click the '_admins' OU that we created and create a new user for our administrator account. Name it and set a password for this user account. After you create this user, you will have to right-click and open properties, inside that menu, go to 'Member Of' and then click Add.. and under the object name, type Domain Admins, and press Check Names and then OK and then Apply. Now, we can sign out out of domain controller administrator account, and sign into the user account that we added to the Domain Admins group. You will have to type in the username and password that you created for that user. <br />
Next, we will setup the Remote Access Service on the domain controller, that way we will be able to access the internet through the domain controller when we create a Windows 11 virtual client, but also remain on the private virtual network. 
For settng up the RAS, we will go to 'add roles and features' on the server dashboard. Click next on the first 2 steps and on the server selection make sure the Domain Controller that you made is selected, click next and then select Remote Access under Features. Click next and under Role Services, you will select Routing and then Add Features, and then click next to advance past the remaining steps. Install the role and you will go to tools in the top right of server manager and select 'Routing and Remote Access'. On the left side, you will right-click your domain controller and select Configure and Enable Routing and Remote Access. Next, select NAT and advance to the next screen and under 'Use this public interface to connect to the Internet', you will select the one named Internet (NOT the internal one). <br />
Next, we will install our DHCP server. You will select Add roles and features on the server manager again. Advance to server selection and select your domain controller and advance to Server Roles where you will select DHCP Server and then advance to installation. When this installation finishes, you will go to DHCP under Tools on the top right of the Server Manager. Now, we will be setting our DHCP scope range for IP addresses that we want to assign to our client computers and then we will install routing on our DHCP server. To set the scope, you will expand the drop-down on the left side under DHCP -> [servername].mydomain.com -> and then right-click 'IPv4' and go to 'New Scope...' to launch the Scope Wizard. In this tutorial, I am naming the Scope Name the range of the scope -> 172.16.0.100-200. Next, you will enter the starting IP Address which would be the first available IP address that the DHCP will assign and then up to the last IP address that we want to be assigned within this range. Starting IP Address: 172.16.0.100 , End IP Address: 172.16.0.200. Under the subnet mask, we will choose /24, which is 255.255.255.0. That way we will have the ability to divide our network into more sub-networks. You don't need to add any exclusions and then you will choose the lease duration which is how long the device can use that IP Address before it needs to be renewed. After half the lease time passes, the device will try to renew the lease with the DHCP server. In this lab, I decided to set it to 8 days since we have no reason to set it to a shorter time. Next, we will add our Default Gateway for the Router on the DHCP server. Our default gateway from the network diagram is 172.16.0.1. Next for DNS, we will use mydomain.com as our parent domain because when you install Active Directory onto a domain controller it automatically installs DNS so our domain controller will be our DNS server. Make sure you add the IP address for the gateway in the Domain Name and DNS Servers options. We are not using WINS Servers so you can leave that blank and advance to Activate Scope and choose Yes, I want to active this scope now and finish the setup wizard. After this, right click the domain controller on the left side drop down menu and Refresh the servers to make sure the setttings take effect. In my tutorial video, I forgot to do this next step until after I created the Client Windows 11 VM, but right now, you should configure the Router option on the DHCP server. For some reason this is not configured during the setup wizard, so you will right-click 'Server Options' under the IPv4 dropdown on the DHCP, and then select 'Configure Options...'. Select 003 Router under available options and then add the IP Address of the default gateway in the IP address box down below and press Add and then Apply. Close this screen and right-click one last time on the Domain Controller at the top of the DHCP downdown menu and refresh one last time to activate changes. <br />
Now, our next step is going to be downloading the PowerShell script file and the names.txt file. I have the files posted on this repository -> <a href="https://www.github.com/justinkeeth/ActiveDirectoryHomeLab/">AD Repository</a>.<br />
Save the files to your desktop or a folder that you know how to navigate to. Launch Windows PowerShell ISE as an administrator and open the 1_CREATE_USERS.ps1 file that we downloaded from the repository above. This script is going to create users and assign them to a new OU (Organizational Unit) called '_USERS' and it will input each user's name and username from the names list in our names.txt file. Since there are so many random names being created, I find it's nice to have a user account created with my own name so I can easily search for it and have a login for a standard user which differs from the admin account that I created earlier. To add your own name to the list of users being created, you will have to open the names.txt file in Notepad and on the first line add your first and last name before the other generated names start. So, this user account with our name will be the first standard user created by the script. The usernames will be made in the format of (first letter of first name + last name), so for example my name is Justin Keeth and the username that will be created for my name will be 'jkeeth'. Make sure to save the names.txt file with your name added before attempting to run the PowerShell script. Next, before we are able to run the create users script file, we have to bypass the security error that will prevent the script from being ran. To bypass this security feature, we will type the following command into the prompt at the bottom of the PowerShell ISE interface: Set-ExecutionPolicy Unrestricted (and press <enter>). It will ask if you are sure you want to change the execution policy and click 'Yes to All'. Now we will be able to run the script with no errors.

</p>



<p></p><h2>Visuals of VirtualBox Setup:</h2>
<div align="center">
 <p><h3>VirtualBox Download preview:</h3></p>
<img width="1073" height="266" alt="virtboxprev" src="https://github.com/user-attachments/assets/2c8e43c6-2646-454d-8c0a-8b617fb01b2c" />
<br />
 <p><h3>VirtualBox Server Setup:</h3></p>
<img width="1055" height="559" alt="Screenshot 2025-11-14 110447" src="https://github.com/user-attachments/assets/19c01b91-b988-4fd2-9c11-f4dca7075a1d" />


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
