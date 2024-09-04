<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<!---
<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com) -->

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 - create a Domain Controller (DC1) in Azure 
- Step 2 - create at least 1 Client VM in the same virtual network in Azure
- Step 3 - set up Active Directory in the Domain Controller (DC1)
- Step 4 - join the Client VM to the same domain

<h2>Deployment and Configuration Steps</h2>

<img src="https://github.com/user-attachments/assets/5c84f6bf-2c26-4600-b3f7-073e5835df38" alt="Azure Active Directory Diagram" />

<p>I will create a VM running Windows Server, set it to static private IP, and install Active Directory. This will be our Domain Controller "DC1".</p>

<p>I will create a client VM running Windows 10. It will later join the same domain as the Domain Controller.  Our result is that any user we have in our DC1 can log in to our client VM, despite never having logged in to the client VM before.  Situation is similar to being at a University Computer Lab in a campus library, or at a midsized IT company housing a number of client computers.</p>

<p>Begin by creating our 2 virtual machines within the same virtual network in Azure. DC1 will represent our Domain Controller running Windows Server.  Client1 will represent 1 of our client computers running Windows 10 Pro.</p>
<img src="https://github.com/user-attachments/assets/de1f101a-8dc7-4f0f-bc05-31560590989a" alt="DC1 and client1 in Azure VM" />

<p>Set DC1 VM to static IP. In Azure, DC1, Network settings, then Network interface ID, then Settings and IP configuration, edit on the right pane to Static:</p>
<img src="https://github.com/user-attachments/assets/d3768293-e196-409f-aa2e-682ecc005d5e" alt="Change DC1 to Static IP" />

<p>Next, using Remote Desktop Connection, log in to our DC1 computer, then open Server Manager (if not opened already), in the top right Manage  menubar select "Add Roles and Features Wizard" and click "Active Directory Domain Services":</p>
<img src="https://github.com/user-attachments/assets/394970e4-d4aa-45e5-8aaa-3f6c03fb2960" alt="Dashboard small" />
<img src="https://github.com/user-attachments/assets/f760e3d2-73ac-4366-9672-38b9d5dc7fcd" alt="Manage menu showing Add Roles and Features" />
<img src="https://github.com/user-attachments/assets/5f3d881a-8624-42be-b2e7-f47c7eb2c33b" alt="Select Active Directory Domain Services" />

<p>Follow the installation wizard:</p>
<img src="https://github.com/user-attachments/assets/76b502e6-c07f-4c2f-9f48-c0d236a050ab" alt="Installing AD" />
<img src="https://github.com/user-attachments/assets/6b429d51-68a5-4f0c-851c-b74dcc5859ec" alt="Confirm installing AD" />
<img src="https://github.com/user-attachments/assets/2cd1ac44-909c-4ead-9771-df01fba337f9" alt="Installation progress" />

<p>Once done installing, the Server Manager Dashboard will have a new notification on the top right. Click on "Promote this server to a domain controller":</p>
<img src="https://github.com/user-attachments/assets/b8c79002-295d-4b24-9e41-fd78c8e48526" alt="Promote this server to a domain controller" />

<p>The following dialog box appears. Select "Add a new forest". In my example, I chose "frankdomain.com" as my test domain.</p>
<img src="https://github.com/user-attachments/assets/d6ca8740-1840-4bd7-885b-4a786404ae79" alt="Add a new forest" />

<p>Enter a Directory Services Restore Mode password:</p>
<img src="https://github.com/user-attachments/assets/7021d46d-b292-4f66-b6d5-ee64876ead26" alt="Domain Controller Options" />

<p>Complete DNS Options. For my test case, as I am using a test domain, I received a warning.</p>
<img src="https://github.com/user-attachments/assets/da86f863-5321-48fd-a3b2-4d744992b784" alt="DNS Options" />

<p>Backward compatibility NetBIOS options. Click Next:</p>
<img src="https://github.com/user-attachments/assets/16f84c98-2565-4ba7-8106-46003c879f2b" alt="NetBios Additional Options" />

<p>Using default for Paths:</p>
<img src="https://github.com/user-attachments/assets/f57f0846-b557-47ed-9bf8-86300525eb4e" alt="Paths" />

<p>Review and finish installing Active Directory:</p>
<img src="https://github.com/user-attachments/assets/e9977d28-5d61-4e15-9626-bc89a4ec4a40" alt="Review Options" />
<img src="https://github.com/user-attachments/assets/f79f6ddf-cf4f-4ab5-8480-1f03d797b1cf" alt="Prerequisites Check" />
<img src="https://github.com/user-attachments/assets/bd6d96f7-d247-4a6e-bcc1-ce5c82c08a59" alt="Waiting for DNS installation to finish" />

<p>Almost done. Computer will need to sign out and restart for Active Directory changes to take effect:</p>
<img src="https://github.com/user-attachments/assets/afb9a39c-bec7-402e-9448-9a567ea9280e" alt="About to restart" />




