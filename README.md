<h1>Active Directory Home Lab</h1>


<h2>Description</h2>
Project consists of setting up Virtual Home Lab on Windows Server 2019 (or 2022) to run Active Directory and then use a Powershell script to create a bulk list of users to assign to the Active Directory. 
<br />


<h2>Software versions and Environments Used</h2>

- <b>VirtualBox by Oracle (version 7.2.4)</b>
- <b>Windows 10/ Windows 11</b>
- <b>Windows Server 2019 or 2022</b>
- <b>PowerShell</b>



<h2>Lab walk-through:</h2>

<p>
 You first download and install VirtualBox by Oracle (You will need to download and install the VirtualBox Extension pack as well). Next step is to download an ISO file for the Windows Server 2019 or 2022 and an ISO file for Windows 10 or Windows 11. You will need available hardware to allocate memory, hard disk, and processing power from the CPU (2-4 cores for adequate processing speed) for the virtual machines to run on. While setting up the virtual machines, you will need to configure the amount of hardware to allocate, and then select the ISO file for the Windows Server version you have downloaded. Before you finish configuring the virtual machine, you will need setup 2 Network Adapters, the first network adapter will be set to NAT for the virtual machine to have access your home internet connection, and the 2nd Network adapter will be for the internal domain network that is being created for the Active Directory environment. Next, you will launch the virtual machine into the OS installation for the server and complete the installation process for the operating sysem onto the VM.
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
