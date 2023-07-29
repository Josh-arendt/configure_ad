# configure_ad
Configuring on-premise Active Directory within Azure VMs
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services


<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 Create a Windows server VM (DC-1) and a Windows VM (Client-1, used to test connectivity/access to Active Directory) within Microsoft Azure.
- Step 2 Assign Active Directory Domain Services to DC-1 and promote server to domain controller.
- Step 3 Create new domain. 
- Step 4 Assign Client-1 to newly created domain.

<h2>Deployment and Configuration Steps</h2>

<p>
  
  ![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/0defb3fb-3c95-4fe1-b111-d3132a1573ce)

</p>
<p>
Creat a new virtual machine inside Microsoft Azure's cloud environment.
<br />
<p>

  ![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/5ecf9472-d756-4dcc-b968-d47152e06adf)
</p>
<p>
Generate a new Resource Group. Name the VM (DC-1 for this exapmple). Select a Windows servers and assign a sufficient ammount of vcpus based on the scale/budget of your project. Assign a username and password. Click review and create.

<br />

<p>

  ![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/69eadc2b-965a-42d1-bb11-5bac7a8c9745)

</p>
<p>
While DC-1 is being deployed, create another VM

<br />

<p>

  ![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/7f26875f-37a3-4949-ae3b-30a38141dbde)


</p>
<p>
Add the new VM to your existing Resource Group. Name the VM (CLient-1 for this example). Select a Windows desktop image. Select a sufficient amount of vcpus based on the scale/budget of your project. Assign a username and password. Click review and create. (You may want to double check the "networking" tab to ensure DC-1 and Client-1 are sharing the same network. This should be done by default but doesn't hurt to check.)

<br />

<p>

  ![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/a3f66acf-a056-4562-8397-37cf450ce157)

</p>
<p>
While Client-1 is being deployed, we will need to assign a static ip to DC-1. To do this, start by selecting DC-1's virtual NIC.

<br />

<p>

  ![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/2041edf7-491d-4dec-865e-dd0d85dc4adb)

</p>
<p>
Select IP configurations.

<br />

<p>

 ![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/e62ab6ad-a512-450f-bfda-2744a7a608fb)

</p>
<p>
Select ipconfig1.

<br />

<p>

![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/cd07e85c-55fa-4782-92b1-50ff70b40c96)

</p>
<p>
Set private IP allocation to "static." Make note of the private ip address for DC-1 (10.0.0.4 for this example).

<br />

<p>

![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/a4fb9741-5ae6-423e-8260-f54acf42a975)

</p>
<p>
Now we must establish connection between DC-1 and Client-1. To do this we will allow inbound ICMP traffic into DC-1 from Client-1. Start by selecting DC-1 VM within Azure.

<br />

<p>

![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/e320d5d6-90a3-4e3f-aee8-05a31f4a5fd6)

</p>
<p>
Copy DC-1's public IP adress.

<br />

<p>

![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/b46df5b0-38cf-4463-bc83-b49201eb5273)

</p>
<p>
Launch Remote Desktop Connection and paste DC-1's public IP address and click connect.
<br />

<p>

![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/9fd7e541-2965-469c-ab04-4f99ed6f1454)

</p>
<p>
Log into DC-1 with the previously created username and passowrd.
<br />

<p>

![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/f6b7227e-6790-4f4a-9e70-e4e67825aa19)

</p>
<p>
Inside DC-1, open Windows Defender Firewall and Advanced Security
<br />

<p>

![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/21437dbe-a346-441a-96f9-ac8808967b6d)

</p>
<p>
Select Inbound Rules.
<br />

<p>

![image](https://github.com/Josh-arendt/configure_ad/assets/140751318/a601a915-2a4c-491a-89e1-8b44740fc031)

</p>
<p>
Sort by protocols and find the cluster of ICMPv4 inbound rules. Right-click and enable the two "Core Networking Diagnostics."
<br />



