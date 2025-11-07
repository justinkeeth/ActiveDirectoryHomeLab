<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
Project consists of setting up Virtual Home Lab on Windows Server 2019 (or 2022) to run Active Directory and then use a Powershell script to create a bulk list of users to assign to the Active Directory. 
<br />


<h2>Software versions and Environments Used</h2>

- <b>VirtualBox by Oracle</b>
- <b>Windows 10 or 11</b>
- <b>Windows Server 2019 or 2022</b>
- <b>PowerShell</b>



<h2>Lab walk-through:</h2>

<p>
 You first download and install VirtualBox by Oracle. Next step is to download an ISO file for the Windows Server 2019 or 2022 and an ISO file for Windows 10 or Windows 11. You will need available hardware to allocate memory, hard disk, and processing power from the CPU (2-4 cores for adequate processing speed) for the virtual machines to run on. While setting up the virtual machines, you will need to configure the amount of hardware to allocate, and then setup the Network Adapters, first you will setup a network for NAT so the virtual machine can access your internet connection, and the 2nd Network adapter will be for the internal domain network that is being created for the Active Directory environment.
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
