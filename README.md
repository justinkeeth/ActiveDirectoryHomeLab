<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
Project consists of setting up Virtual Home Lab on Windows Server 2025 to run Active Directory and then use a Powershell script to create a bulk list of users to assign to the Active Directory. [The script file and user names txt file were uploaded and attached to this repository, you will want to download them onto the virtual server that is created. -> ('1_CREATE_USERS.ps1' PowerShell script and 'names.txt')].
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
 <h3>Part 1: Prerequisites & Downloads</h3>

Download and install <a href="https://www.virtualbox.org/wiki/Downloads"> Oracle VirtualBox</a>.

Download and install the VirtualBox Extension Pack (can be found on the same page as the VirtualBox download and must match your VirtualBox version).

Download the following ISO files:

<a href="https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025">Windows Server 2025</a>

<a href="https://www.microsoft.com/en-us/software-download/windows11">Windows 11 (Pro)</a> (You cannot join a domain with Windows 11 Home).

Ensure your host computer has sufficient resources: (You will need the following available resources to be allocated to run smoothly)

CPU: 2–4+ cores recommended

RAM: At least 4 GB available to allocate (more is better)

Storage: At least 50 GB free

<h3>Part 2: Create the Windows Server 2025 Virtual Machine</h3>

Open VirtualBox and create a new VM.

Assign the following:

Name: (Your choice of what you would like to name the server VM)

Iso: Select the Windows Server 2025 iso (that you downloaded)

Edition: Windows Server 2025 Standard Evaluation (Desktop Experience)

Type: Microsoft Windows

Version: Windows Server 2025 (64-bit)

Allocate hardware resources (RAM, CPU, disk).

Make sure to check the box to select: Skip Unattended Installation

Configure 2 Network Adapters (1 for the internet connection that our computer is using and 1 for the internal network that we are going to create).

Adapter 1: Attached to: NAT , Purpose: Internet access.

Adapter 2: Attached to: Internal Network (Name appropriately) , Purpose: Active Directory domain network.

<h3>Part 3: Install Windows Server 2025</h3>

Start the VM and begin installation.

Select Windows Server with Desktop Experience (GUI is required, otherwise there will only be a command prompt).

Complete the OS installation.

Log in as the local Administrator. (I prefer to use Password1 in my homelabs to make it easy to remember)

Install VirtualBox Guest Additions and we will manually reboot at the end of Part 4.

<h3>Part 4: Configure Network Settings on the Server</h3>
Identify Network Adapters

Open Network Connections.

Identify each adapter by checking Status → Details.

NAT adapter: Home network IP

Internal adapter: This will have an APIPA address (169.254.x.x)

Rename Adapters:

Rename NAT adapter to: "Internet"

Rename internal adapter to: "Internal" or "Domain Network"

Configure the Internal Adapter IPv4 Properties.

Open IPv4 Properties for the Internal adapter.

Configure:

IP Address: 172.16.0.1

Subnet Mask: 255.255.255.0

Default Gateway: (leave blank)

Preferred DNS Server: 127.0.0.1 (Loopback IP address, the domain controller will serve as our DNS Server).

Rename the server to DomainController within Windows Settings and reboot.

<h3>Part 5: Install Active Directory Domain Services (AD DS)</h3>

Open Server Manager → Add Roles and Features.

Select the local server.

Install Active Directory Domain Services.

Promote the server to a domain controller.

Create a new forest:

Domain name: mydomain.com

Complete the wizard and reboot.

<h3>Part 6: Create Admin Organizational Unit & User</h3>

Open Active Directory Users and Computers.

Right-click mydomain.com → New → Organizational Unit.

Name the OU: _admins.

Right-click _admins → New → User.

Create an admin user account.

Open user properties → Member Of tab.

Add the user to Domain Admins.

Sign out and log back in using this new domain admin account.

<h3>Part 7: Configure Routing and Remote Access (NAT)</h3>

Open Server Manager → Add Roles and Features.

Install Remote Access.

Under Role Services, select Routing.

Open Tools → Routing and Remote Access.

Right-click the server → Configure and Enable Routing and Remote Access.

Select NAT.

Choose the Internet adapter as the public interface.

<h3>Part 8: Install and Configure DHCP Server</h3>
Install DHCP

Add Roles and Features → Install DHCP Server.

Complete installation.

Create DHCP Scope

Open Tools → DHCP.

Navigate to:

DHCP → Server → IPv4

Right-click IPv4 → New Scope.

Configure:

Scope Name: 172.16.0.100-200

Start IP: 172.16.0.100

End IP: 172.16.0.200

Subnet Mask: 255.255.255.0

Lease Duration: 8 days

Default Gateway: 172.16.0.1

DNS:

Domain: mydomain.com

DNS Server IP: 172.16.0.1

Activate the scope.

Configure Router Option

Right-click Server Options → Configure Options.

Enable: 003 Router.

Add IP: 172.16.0.1.

Apply and refresh the server.

<h3>Part 9: Create Users with PowerShell Script</h3>

Download 1_CREATE_USERS.ps1 and names.txt from the AD repository.

Save files locally.

Edit names.txt and add your name at the top.

Open PowerShell ISE as Administrator.

Run: Set-ExecutionPolicy Unrestricted

Open and run 1_CREATE_USERS.ps1.

Verify users appear in the _USERS OU.

<h3>Part 10: Create Windows 11 Client VM</h3>

Create a new VM in VirtualBox.

Select Windows 11 Pro ISO.

Skip unattended installation.

Allocate resources meeting Windows 11 requirements.

Set network adapter to Internal Network.

Windows 11 Setup (No Internet)

During setup, press Shift + F10 (This following command will allow us to bypass the Network Requirement to be Online).

Run: OOBE\BYPASSNRO

Continue installation offline.

<h3>Part 11: Join Windows 11 Client to Domain</h3>

Rename the computer and reboot.

Open Settings → System → About.

Select Domain or workgroup → Change.

Join domain: mydomain.com.

Enter domain admin credentials.

Reboot when prompted.

<h3>Part 12: Verification & Testing</h3>

Run ipconfig on the client: IPv4 Address should be in 172.16.0.100–200 range.

Open Active Directory Users and Computers.

Verify the client appears in the Computers OU.

Log into the Windows 11 client using a domain user account (not the admin account).

<h3>Lab Outcome:</h3>

You now have a functional Active Directory lab with:

- Domain Controller

- NAT & internal networking

- DHCP-managed clients

- Automated user creation

- Domain-joined Windows 11 client

This lab closely simulates a real-world enterprise environment and provides hands-on experience with core Windows Server administration tasks. </p>
<p>Also, just wanted to note if you are watching the YouTube video while reading this walk-through guide, I forgot to enable the 003 Router on the DHCP server when I setup the DHCP earlier on the Server VM, so I performed this step at 33:22 time on the video. <br>
I accidentally disabled the 'cable connected' checkbox on the Virtualbox settings of the Windows 11 Pro VM while the OS was installing. I had to troubleshoot why the ipconfig was showing ethernet disconnected, and solved the issue by enabling Cable Connected within the Network Adapter settings on Virtualbox for that client VM. (This was performed at 35:15 on the video)
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
