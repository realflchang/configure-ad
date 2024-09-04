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
<br /><br />

<p>I will create a client VM running Windows 10. It will later join the same domain as the Domain Controller.  Our result is that any user we have in our DC1 can log in to our client VM, despite never having logged in to the client VM before.  Situation is similar to being at a University Computer Lab in a campus library, or at a midsized IT company housing a number of client computers.</p>
<br /><br />

<p>Begin by creating our 2 virtual machines within the same virtual network in Azure. DC1 will represent our Domain Controller running Windows Server.  Client1 will represent 1 of our client computers running Windows 10 Pro.</p>
<img src="https://github.com/user-attachments/assets/de1f101a-8dc7-4f0f-bc05-31560590989a" alt="DC1 and client1 in Azure VM" />
<br /><br />

<p>Set DC1 VM to static IP. In Azure, DC1, Network settings, then Network interface ID, then Settings and IP configuration, edit on the right pane to Static:</p>
<img src="https://github.com/user-attachments/assets/d3768293-e196-409f-aa2e-682ecc005d5e" alt="Change DC1 to Static IP" />
<br /><br />

<p>Next, using Remote Desktop Connection, log in to our DC1 computer, then open Server Manager (if not opened already), in the top right Manage  menubar select "Add Roles and Features Wizard" and click "Active Directory Domain Services":</p>
<img src="https://github.com/user-attachments/assets/394970e4-d4aa-45e5-8aaa-3f6c03fb2960" alt="Dashboard small" />
<img src="https://github.com/user-attachments/assets/f760e3d2-73ac-4366-9672-38b9d5dc7fcd" alt="Manage menu showing Add Roles and Features" />
<img src="https://github.com/user-attachments/assets/5f3d881a-8624-42be-b2e7-f47c7eb2c33b" alt="Select Active Directory Domain Services" />
<br /><br />

<p>Follow the installation wizard:</p>
<img src="https://github.com/user-attachments/assets/76b502e6-c07f-4c2f-9f48-c0d236a050ab" alt="Installing AD" />
<img src="https://github.com/user-attachments/assets/6b429d51-68a5-4f0c-851c-b74dcc5859ec" alt="Confirm installing AD" />
<img src="https://github.com/user-attachments/assets/2cd1ac44-909c-4ead-9771-df01fba337f9" alt="Installation progress" />
<br /><br />

<p>Once done installing, the Server Manager Dashboard will have a new notification on the top right. Click on "Promote this server to a domain controller":</p>
<img src="https://github.com/user-attachments/assets/b8c79002-295d-4b24-9e41-fd78c8e48526" alt="Promote this server to a domain controller" />
<br /><br />

<p>The following dialog box appears. Select "Add a new forest". In my example, I chose "frankdomain.com" as my test domain.</p>
<img src="https://github.com/user-attachments/assets/d6ca8740-1840-4bd7-885b-4a786404ae79" alt="Add a new forest" />
<br /><br />

<p>Enter a Directory Services Restore Mode password:</p>
<img src="https://github.com/user-attachments/assets/7021d46d-b292-4f66-b6d5-ee64876ead26" alt="Domain Controller Options" />
<br /><br />

<p>Complete DNS Options. For my test case, as I am using a test domain, I received a warning.</p>
<img src="https://github.com/user-attachments/assets/da86f863-5321-48fd-a3b2-4d744992b784" alt="DNS Options" />
<br /><br />

<p>Backward compatibility NetBIOS options. Click Next:</p>
<img src="https://github.com/user-attachments/assets/16f84c98-2565-4ba7-8106-46003c879f2b" alt="NetBios Additional Options" />
<br /><br />

<p>Using default for Paths:</p>
<img src="https://github.com/user-attachments/assets/f57f0846-b557-47ed-9bf8-86300525eb4e" alt="Paths" />
<br /><br />

<p>Review and finish installing Active Directory:</p>
<img src="https://github.com/user-attachments/assets/e9977d28-5d61-4e15-9626-bc89a4ec4a40" alt="Review Options" />
<img src="https://github.com/user-attachments/assets/f79f6ddf-cf4f-4ab5-8480-1f03d797b1cf" alt="Prerequisites Check" />
<img src="https://github.com/user-attachments/assets/bd6d96f7-d247-4a6e-bcc1-ce5c82c08a59" alt="Waiting for DNS installation to finish" />
<br /><br />

<p>Almost done. Computer will need to sign out and restart for Active Directory changes to take effect:</p>
<img src="https://github.com/user-attachments/assets/afb9a39c-bec7-402e-9448-9a567ea9280e" alt="About to restart" />
<br /><br />

<p>When logging back in to DC1, this time we use the format domain\username, since it now has DC set up. I signed back in as "frankdomain.com\rootuser". I get a message to wait for the Group Policy Client:</p>
<img src="https://github.com/user-attachments/assets/1a45fd3e-3823-47fd-80f1-6fdcef1fd731" alt="Please wait for the Group Policy Client" />
<br /><br />

<p>After it is ready, go in Active Directory Users and Computers (ADUC), create Organizational Units (OU) called “_EMPLOYEES” & “_ADMINS”. </p>
<img src="https://github.com/user-attachments/assets/25152422-db2f-4ce9-b340-5b4a97830670" alt="ADUC Menu" />
<img src="https://github.com/user-attachments/assets/f5f6d1b0-e47a-4698-ae75-7eec343519b7" alt="Organizational Units Admins and Employees" />
<br /><br />

<p>Create a domain user, and add it to the "Domain Admins" group. In my case I call the user "fc_admin":</p>
<img src="https://github.com/user-attachments/assets/2ffb2815-ca0d-4ce1-8222-7f319d4fbfb9" alt="Add domain user fc_admin" />
<img src="https://github.com/user-attachments/assets/c20c09f6-4814-4e0a-85bc-e30b42bde741" alt="Add to Domain Admins" />
<br /><br />

<p>Sign out of our default DC1 admin account. Best practice from now to just use the account associated with Domain Admins:</p>
<img src="https://github.com/user-attachments/assets/df4eddce-5010-4416-adbc-58549df0b3fb" alt="Signing out to sign back in as Domain Admin" />
<br /><br />

<p>In Azure portal, we now set Client1 VM DNS to DC1’s private IP address to join its domain. Go to the Client VM's Network settings, then Network interface ID:</p>
<img src="https://github.com/user-attachments/assets/3d3b70c5-3c89-4f2f-8b50-972484699708" alt="Client Network settings" />
<br /><br />

<p>Next, under DNS servers, we click on "Custom" and add the private IP of the DC1 Domain Controller. In my case it is 10.0.0.4:</p>
<img src="https://github.com/user-attachments/assets/39f325fb-dfe7-4537-927b-a293b9625ba5" alt="DNS Server, click on Custom" />
<img src="https://github.com/user-attachments/assets/ea3c8926-d899-4da5-af30-abd6de3fab60" alt="Put in the Private IP of the DC" />
<img src="https://github.com/user-attachments/assets/6248aff6-ff51-452a-a8a8-3e92c9c36d5a" alt="Click Save" />
<br /><br />

<p>Restart our Client VM in Azure, and sign back in. In command prompt, running ipconfig /all now shows DNS Server pointing to our DC1:</p>
<img src="https://github.com/user-attachments/assets/b98f7a43-7b95-4ed3-bcb4-9969b1f43c75" alt="Client VM now has DNS to DC" />
<br /><br />

<p>Let's make the Client VM join the DC1 domain. In System properties, click on "To rename this computer or change its domain or workgroup:</p>
<img src="https://github.com/user-attachments/assets/f0724fc1-22a9-4621-a3d2-43b46b4ae388" alt="System properties" />
<br /><br />

<p>Under Member of, put in Domain:</p>
<img src="https://github.com/user-attachments/assets/dfaa5edf-9a7f-4b5d-8de9-713c4fa28003" alt="Member of Domain" />
<br /><br />

<p>Client VM now requests a new username and password to use when in the domain:</p>
<img src="https://github.com/user-attachments/assets/30f59259-b226-4cc4-8daf-bba016098c44" alt="Provide new username and password for use in the domain" />
<br /><br />

<p>Welcome and restart computer messages appear:</p>
<img src="https://github.com/user-attachments/assets/acdffbf9-959b-4307-b2e6-bfe0f4836887" alt="Welcome" />
<img src="https://github.com/user-attachments/assets/e5b5c5b2-581d-42f5-8a76-0162536cda26" alt="Must restart" />
<img src="https://github.com/user-attachments/assets/24691e22-f70a-4abc-b8d2-3b1970022c51" alt="Save work before restarting" />
<br /><br />

<p>As Client VM restarts, going back to our DC1 Server Manager, Active Directory Users and Computers, under "Computers" we now see our client computer listed:</p>
<img src="https://github.com/user-attachments/assets/68b12862-5ca8-4c71-9b91-ba1f62a00e8a" alt="Client computer now appears in Active Directory" />
<br /><br />

<p>Moving the client to a new Organizational Unit called "_CLIENTS" for organizational purposes, in Active Directory.</p>
<img src="https://github.com/user-attachments/assets/4b4017f8-f35f-4a9e-9092-b775dec00794" alt="Moving to _CLIENTS Organizational Unit" />
<br /><br />

<p>Back to our Client VM, sign back using frankdomain.com\fc_admin, in the Settings, Remote Desktop, allow other domain users to remote into it:</p>
<img src="https://github.com/user-attachments/assets/75ec4088-b102-4961-b62f-f74e35350d25" alt="Remote Desktop PC settings" />
<img src="https://github.com/user-attachments/assets/3b6fdb7d-c864-429d-b0da-3a04cbfc4a4e" alt="Remote Desktop Users blank" />
<img src="https://github.com/user-attachments/assets/c11ced14-00ff-498a-b80a-abc7a482ae0d" alt="Remote Desktop Users adding Domain Users" />
<img src="https://github.com/user-attachments/assets/9284947c-c2e3-4db7-9542-e373ccb1f9c3" alt="Remote Desktop Users showing Domain Users" />
<br /><br />

<p>Back to our DC1, Server Manager, let's create our users. In the real world, these would be our employees in a company or students in an university:</p>
<img src="https://github.com/user-attachments/assets/f694829d-df16-4e37-91ab-90261a92ee39" alt="Creating sample users" />
<br /><br />

<p>Back to our Client VM, sign out and try signing in as an ordinary user. Let's try "vuv.mapo" in my case:</p>
<img src="https://github.com/user-attachments/assets/4aaabe4c-bb3f-47d3-a76f-70f58fb4a8d5" alt="Signing in to Client as vuv.mapo" />
<img src="https://github.com/user-attachments/assets/0e91a43b-77b1-4489-974c-f103818ec711" alt="Welcome vuv.mapo" />
<img src="https://github.com/user-attachments/assets/c2a6b0e1-1b73-4817-94b0-8f0195d0d5f9" alt="Command prompt showing vuv.mapo" />
<br /><br />

<p>This confirms Client VM is accessible for any user from Domain Users group in the same domain as the Domain Controller.</p>
<br /><br />

<p>Note that in our Client VM, although we pointed the DNS to our DC1, the computer can still resolve other domains. For example, I can still go to google.com or cnn.com. This is due to the DNS in DC having "Root Hints" with default DNS servers, in case the regular DNS in DC1 cannot resolve for client VM:</p>
<img src="https://github.com/user-attachments/assets/32046a09-a095-4539-97ee-cfdeb94c694e" alt="Server Manager, DNS" />
<img src="https://github.com/user-attachments/assets/6f4d81d7-d052-4682-b94d-2d39ff066770" alt="Root Hints default servers" />
<br /><br />

<p>Server Manager has other options available, such as Tools, Group Policy Management to easily configure users and computers as a group:</p>
<img src="https://github.com/user-attachments/assets/e95612f3-441f-4969-ab11-d6c72c597428" alt="Manage menu" />
<img src="https://github.com/user-attachments/assets/8040b73b-c698-438a-800f-78b8da008b9d" alt="Tools menu" />




