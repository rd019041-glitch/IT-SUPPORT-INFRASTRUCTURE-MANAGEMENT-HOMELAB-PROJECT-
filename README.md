# IT-SUPPORT-INFRASTRUCTURE-MANAGEMENT-HOMELAB-PROJECT-
This project is a comprehensive home lab environment designed to simulate real-world IT Support and System Administration tasks following ITSM practices. It demonstrates the end-to-end setup, configuration, and management of an enterprise-like IT infrastructure, with a strong focus on identity management, security, automation, and troubleshooting.
Welcome to my comprehensive IT infrastructure project, where I have meticulously designed and implemented a home lab environment integrating Active Directory solutions, both on-premises and in the cloud through Azure Active Directory. This project highlights my proficiency in deploying and managing complex IT systems, utilizing a range of tools and skills to ensure robust operational capabilities.
This project not only showcases my technical expertise in IT infrastructure management but also underscores my ability to design, deploy, and optimize enterprise-grade solutions. It reflects my commitment to excellence in IT service delivery and my readiness to contribute effectively to organizational IT operation.



Table of Contents


•	Home lab Creation
•	User and Group Creation
•	Microsoft 365 Integration and Management
•	Group Policy Management and Implementation
•	Password Reset
•	Multi-factor Authentication
•	Software Deployment Through PDQ Inventory
•	Remote Access and Troubleshooting
•	Task Automation with PowerShell
•	Project Conclusion



	Home lab Creation

<img width="799" height="654" alt="image" src="https://github.com/user-attachments/assets/e114af89-2fe3-4fc4-8815-938fc69cc105" />
1.	Two Servers:
o	One server (Windows Server 2022) will act as the Domain Controller (DC), responsible for managing the Active Directory (AD) and handling authentication and authorization within the domain.
o	The second server (Windows Server 2022) will be used for other purposes such as file storage, applications, or additional services required within the network.
2.	Four Client Machines:
o	These machines will be used to simulate end-user workstations within the network. Each machine will be joined to the domain controlled by the Domain Controller.
o	Naming Convention
	Prefix: JSS
	Client Identifier: PC
	Unique Number: 02, 03, 04,
	Detailed Naming List:
	JSS- PC01 {Helpdesk}
	JSS-PC02
	JSS -PC03
	JSS - PC04 

Organizational Units (OUs) for Departments
To keep our domain structure well-organized, we will create Organizational Units (OUs) for each department. This will help us manage users, computers, and other resources efficiently within Active Directory. The following OUs will be created:
<img width="939" height="631" alt="image" src="https://github.com/user-attachments/assets/4645534d-d727-4dda-b570-1d0ce705b0d7" />

1.	IT - Contains users such as IT Support, Sysadmins, and Network Engineers.
2.	Finance - Contains users such as Finance Managers and Finance Analysts.
3.	Sales - Contains Sales Representatives.
4.	HR - Contains the HR Manager and related personnel.
5.	Marketing - Contains Marketing Specialists and Content Writers.
6.	Development - Contains Developers and DevOps Engineers.
7.	Customer Service - Contains Customer Service Representatives.
8.	Design - Contains Graphic Designers.
9.	Administration - Contains the Office Manager and administrative staff. 


1. Set up Hyper-V on your host machine	



       1.1	Install Hyper-V: Open the Control Panel, navigate to Programs > Programs and Features > Turn Windows features on or off, check the box for Hyper-V, click OK, and restart your computer.


<img width="949" height="540" alt="image" src="https://github.com/user-attachments/assets/5baaef52-27b0-4426-a176-8da1853364bd" />


1.2

	Configure Hyper-V: Open Hyper-V Manager from the Start menu, select your host machine, and configure virtual switches by going to Virtual Switch Manager in the right-hand Actions menu. Create a new External virtual switch to allow VMs to access your physical network.
 <img width="923" height="519" alt="image" src="https://github.com/user-attachments/assets/6f4268a4-d414-4cae-94d0-a67dc8f675a9" />



2. Create Virtual Machines
	Create the Domain Controller VM: In Hyper-V Manager, click New > Virtual Machine, and follow the wizard to set up the VM with appropriate settings such as name, generation, memory, network, and virtual hard disk. Install an operating system later.
In my case, I'm going to choose to change the default path to store the virtual machine, as it seems more organized to me. In this case, I'll opt to place it in C:\Hyper-V\VMs\.
<img width="878" height="635" alt="image" src="https://github.com/user-attachments/assets/efaf83c0-8ff5-4977-8eea-fdeaceaab207" />

Afterwards, I select Generation 2, as Generation 1 can be prone to errors.
 
<img width="963" height="578" alt="image" src="https://github.com/user-attachments/assets/509d3da7-efe1-43ca-9a69-ab52931e6cf4" />

	Now, I need to allocate the RAM to the virtual machine. In this case, I'll opt for the minimum recommended for Windows Server 2022 with Desktop Experience, which is 4GB (4096 MB).
  
Additionally, I'll choose to leave the 'Use dynamic memory for this virtual machine' checkbox checked.
When a virtual machine uses dynamic memory, it means that its memory allocation can adjust automatically based on the demand from the operating system and applications running within the virtual machine. Instead of assigning a fixed amount of memory, the virtual machine can request and release memory as needed, allowing for more efficient utilization of host resources.
<img width="966" height="494" alt="image" src="https://github.com/user-attachments/assets/99f8896b-17c1-45c2-a5cb-2139b4c423c5" />


Then, I select the network adapter to which the virtual machine will connect. In this case, it's the network adapter created in step 1.2 (JSS), which is associated with my Ethernet.
<img width="885" height="575" alt="image" src="https://github.com/user-attachments/assets/3433dac0-b333-41ca-9c91-3e96bc6318ef" />

Next, I will create a virtual hard disk o 127GB, which will be more than enough for any practice you may want to perform.
<img width="872" height="473" alt="image" src="https://github.com/user-attachments/assets/7c719c22-0b13-4d33-b89a-38745b36f211" />

Finally, I select the option 'Install an operating system from a bootable CD/DVD-ROM' and locate the location of the .ISO file containing the Windows Server 2022 Operating System.

<img width="874" height="663" alt="image" src="https://github.com/user-attachments/assets/f8061c23-d4c4-416e-b358-396801aa2b75" />


Once this is done, the virtual machine is successfully created. Now, all that's left is to connect to it, start it up, and follow the Windows Server installer.

<img width="869" height="120" alt="image" src="https://github.com/user-attachments/assets/6c6b73e0-2976-4dee-96eb-99f44d9247ab" />

Create the Second Server VM: Follow the same steps as above but name this VM Server2.
<img width="874" height="397" alt="image" src="https://github.com/user-attachments/assets/24582605-a177-4a1b-9695-120e2f9043e7" />

Next, I create the second virtual machine, which will also have Windows Server 2022 as its operating system. I will avoid detailing the installation as it is the same as the DC01 virtual machine, only the name is different.
In this case, the virtual machine will be named 'SV02'.

Create the Client VMs: Repeat the VM creation process four times for client machines, naming them JSS-PC01, JSS-PC02, JSS-PC03, and JSS-PC04. Assign at least 3024 MB of memory to each client VM.

Now, I repeat the process but instead of using the Windows Server 2022 .iso file as the boot disk, I choose the Windows 10 .iso file.
<img width="402" height="321" alt="image" src="https://github.com/user-attachments/assets/18084d54-1a4b-4127-b6a2-0f9d3dd82518" />
<img width="393" height="306" alt="image" src="https://github.com/user-attachments/assets/9e6fceff-a799-4f54-95a0-b1d5348ab664" />


Then, once the 2 servers (Windows Server 2022) and the four client machines (Windows 10) are created, I have the following:
<img width="946" height="264" alt="image" src="https://github.com/user-attachments/assets/fc06e418-996d-4322-bd7a-d7e24dd3bbb6" />

3. Install the necessary operating systems on all VMs	
3.1	Prepare installation media: Obtain ISO files for Windows Server (for the servers) and Windows 10/11 (for the client machines).

In this case, I chose to download both .iso files (Windows Server 2022 and Windows 10) beforehand, but you can also choose the option 'Install an operating system later,' which creates the virtual machine and waits for you to attach the desired .iso file later.

If you prefer to do it that way, you can download the .iso files from the official Microsoft website:

- Windows Server 2019
- Windows 10

Install Windows Server on DC1 and Server2: Start the VM, connect to it, attach the Windows Server ISO, and follow the installation prompts to install Windows Server. Configure the server with appropriate settings (e.g., server name, IP address).
Once the virtual machine is created and the Windows Server 2022 disk is 'inserted,' I will start it and proceed with the installation. I will perform this process for both the DC01 (Domain Controller) server and the SV02 server

<img width="770" height="394" alt="image" src="https://github.com/user-attachments/assets/1ba39b1d-2bb4-41b2-ac12-5b4f42529980" />
. Click Install..
<img width="684" height="538" alt="image" src="https://github.com/user-attachments/assets/b1cd7721-be60-425e-b34c-e4d470e47c61" />

After clicking 'Install,' I need to select the version I want. Among the options are the Standard and Datacenter versions, each with a Desktop Experience (GUI) version and a Server Core version. In this case, I will select the Standard version with Desktop Experience:
<img width="554" height="378" alt="image" src="https://github.com/user-attachments/assets/75cf1b1e-deb3-4041-87fd-d7a9047c9cb6" />

Accept The license
<img width="566" height="346" alt="image" src="https://github.com/user-attachments/assets/e9632563-13a7-4f55-a022-8031c1bd6932" />

Next, I need to select the partition on which the operating system will be installed. In this case, it is the only partition I have: the 30GB partition created when I created the virtual machine, but I could also create another partition and install it there.
 <img width="593" height="445" alt="image" src="https://github.com/user-attachments/assets/e3d8486f-42eb-49ae-a0c4-1b9a3dbeefff" />

After selecting the partition, the installer will continue copying files and installing the operating system. The installation time varies depending on the resources, both those we have and those allocated to the virtual machine.
 <img width="640" height="478" alt="image" src="https://github.com/user-attachments/assets/53d706a5-7c96-43ba-8d54-52c2ccf15c9d" />


Now, once the above is completed, you will be prompted to create a password for the 'Administrator' user, who has full control over the operating system. This account is similar to the superuser in Linux. It is essential to remember this password.

<img width="950" height="693" alt="image" src="https://github.com/user-attachments/assets/ae135e0a-968f-4e4c-964a-0d2c31c2a517" />


 
Now, I can log in with the credentials provided recently.

 
<img width="952" height="610" alt="image" src="https://github.com/user-attachments/assets/e792bce3-a8eb-4b7b-bd5e-7d4ee6ee6947" />

 <img width="1016" height="502" alt="image" src="https://github.com/user-attachments/assets/1f1e68ab-dc1e-44d3-8a05-65f937b8db56" />


Once logged in, the 'Server Manager' will automatically open (this can be disabled), which allows managing Roles and Features, configuring the server's Firewall, changing the name, changing the IP address, managing storage, etc.

In this case, within the 'Local Server' section, I will change its name and IP address. The IP address will change from being managed by DHCP to static, which is seen as a good practice (in servers, not in client machines), as it increases reliability among other reasons.
<img width="1063" height="592" alt="image" src="https://github.com/user-attachments/assets/6ed298a5-22c2-4993-afba-0ed427435c0b" />
The name will be: DC01.
 <img width="828" height="490" alt="image" src="https://github.com/user-attachments/assets/ff00b85e-9066-4f8c-a729-938ca7c05dac" />

The static IP address for DC01 will be: 192.168.0.1
The subnet mask will be: 255.255.255.0, which means that the first 3 octets of the IP address (192.168.0) will correspond to identifying the network portion, while the fourth and last octet will correspond to identifying the host portion. In this case, with the /24 subnet mask, we get 2^8 - 2 (254) available hosts for the devices and/or servers (2 raised to the available bits, minus 2 because there are 2 addresses that cannot be assigned: 192.168.0.1, which corresponds to the network address, and 192.168.0.255, which corresponds to the broadcast address).
The default gateway will be 192.168.0.1, corresponding to the IP address of the default gateway that I have.
The primary DNS server will be: 8.8.8.8, corresponding to Google's public DNS server.
The alternate DNS server will be: 8.8.4.4, also corresponding to another public DNS server provided by Google.

<img width="840" height="568" alt="image" src="https://github.com/user-attachments/assets/396b98ca-7b4b-4b6a-9d61-d6a5a351dc2d" />











To apply the changes (the server’s name change), it is necessary to restart the computer. That’s all for the DC01 server (temporarily, although it's still not a domain controller). Now, I will proceed to install the Windows Server operating system on the other server machine (SV02).
Install Windows 10/11 on client VMs
Start each client VM, connect to it, attach the Windows 10/11 ISO, and follow the installation prompts to install Windows. Configure each client with appropriate settings (e.g., machine name, IP address). Once the Windows 10 disk is 'inserted' into the client machine, I proceed to start it to initiate the installation process.

<img width="806" height="614" alt="image" src="https://github.com/user-attachments/assets/f7108011-5405-43e6-9790-f0be2f0e838b" />















	Install Now


<img width="660" height="512" alt="image" src="https://github.com/user-attachments/assets/0c93fd52-3a22-4fb9-a4ac-28dbee9c99a4" />












	Accept The License Terms


<img width="842" height="530" alt="image" src="https://github.com/user-attachments/assets/273c1369-de3b-481c-83c3-172459ffbd7c" />



















	Then Choose Custom and Allocate the Hard drive to Install Windows

<img width="950" height="674" alt="image" src="https://github.com/user-attachments/assets/9e6a71a6-2eff-4711-b901-6fa32edb8408" />























	Now, I must wait for the installation process to finish.


<img width="744" height="581" alt="image" src="https://github.com/user-attachments/assets/d0a45285-b6e2-484d-8979-9de8605465a1" />






<img width="805" height="582" alt="image" src="https://github.com/user-attachments/assets/518ad977-929d-43ae-b705-4c0ac7a1db8c" />













<img width="897" height="637" alt="image" src="https://github.com/user-attachments/assets/7953787e-1a7b-49f9-ad08-571e799fa596" />










<img width="917" height="684" alt="image" src="https://github.com/user-attachments/assets/f77d01ee-eb19-408a-a534-d3d5ea986eaa" />











<img width="827" height="585" alt="image" src="https://github.com/user-attachments/assets/84348ed4-2069-4b55-88ac-86543edfeb4f" />

































That's it. Now, once the operating system has started, I complete the final configurations (region, keyboard, network, local account, privacy, etc.), and once the desktop is displayed, I proceed to change the computer name to "JSS-PC02":








<img width="814" height="624" alt="image" src="https://github.com/user-attachments/assets/b41c3f26-f7f5-4af6-ad14-b07e7afca359" />







Finally, I repeat the process for the rest of the computers, only varying the name (JMFSOFT-PC01, JMFSOFT-PC03, and JMFSOFT-PC04).
3.	Configure the Domain Controller

Install Active Directory Domain Services (AD DS): Log in to DC1, open Server Manager, click Add roles and features, select Active Directory Domain Services, and complete the installation.

Now, to be able to use the Windows Directory Service (Active Directory), I need to install Active Directory Domain Services (AD DS) on the server designated as the domain controller. To do this, I go to the Server Manager and select the 'Add Roles and Features' option. Then, I select Active Directory Domain Services (AD DS) and follow the steps.




<img width="1016" height="516" alt="image" src="https://github.com/user-attachments/assets/723ae517-c05d-4f18-8e95-ba289baddfbe" />




<img width="978" height="530" alt="image" src="https://github.com/user-attachments/assets/070aba9f-4e41-4bd6-97d1-79ec5a602766" />




<img width="1015" height="728" alt="image" src="https://github.com/user-attachments/assets/48564e4e-bc34-4911-b88f-1f585a39aac9" />



<img width="1039" height="662" alt="image" src="https://github.com/user-attachments/assets/5c2c52bb-bf4b-4021-8549-1106338f37ca" />



<img width="834" height="464" alt="image" src="https://github.com/user-attachments/assets/bce202cc-97f4-4c27-bbbd-2ed23c09aa57" />



<img width="860" height="526" alt="image" src="https://github.com/user-attachments/assets/ddd20dfb-4560-4700-833e-1257f8120b55" />




<img width="844" height="404" alt="image" src="https://github.com/user-attachments/assets/f10df5c5-0f26-4d88-a52b-145c0e42d1da" />







Finally, a summary of the installed roles and features will appear. I can also check the box indicating to the server that it should restart if necessary to complete the installation. This box should be checked carefully, as it is not appropriate to restart a server that is in operation in a business environment unless this restart has been scheduled in advance and does not affect the proper operation of any service. Once verified that everything is correct, I press 'Install' to begin the installation of AD DS.




<img width="854" height="441" alt="image" src="https://github.com/user-attachments/assets/2551c8ed-f046-4cbe-be36-4c5abcbce84c" />








Once this is done, the installation will begin. I can close the window and let the installation run in the background or simply wait for it to finish.



<img width="850" height="550" alt="image" src="https://github.com/user-attachments/assets/9e003c1c-42e8-47e6-8946-e7d4e1470c32" />











Finally, the installation is complete, but this doesn't mean that the server is already a Domain Controller, as only the Active Directory Domain Services (AD DS) were installed. To accomplish this, I must promote the server to a Domain Controller (DC), which will be done in the next step.



<img width="854" height="434" alt="image" src="https://github.com/user-attachments/assets/8f45d162-f988-4a4a-a011-e2a796ff2134" />







Promote DC1 to a Domain Controller: After installation, click on the notification flag in Server Manager, select Promote this server to a domain controller, choose Add a new forest, enter a root domain name (e.g., jss.com), and complete the wizard. Restart the server as prompted.Now, once Active Directory Domain Services are installed, I need to promote the server to a domain controller to create the forest, the domain, and then add the desired users. To do this, I need to go to the notification that appears in the Server Manager and click on 'Promote this server to a domain controller.'





<img width="870" height="550" alt="image" src="https://github.com/user-attachments/assets/be699b8e-709e-4520-a9e7-c029846c183f" />









Once that option is selected, a window will open with three options:
1 - Add a domain controller to an existing domain: This option is useful when you want to add an additional server that provides authentication and authorization services within the same domain (a previously created domain). This increases redundancy and availability. It also helps distribute the load among multiple Domain Controllers, improving performance.
2 - Add a new domain to an existing forest: This involves creating an additional domain within an Active Directory Forest. A forest is a collection of one or more domains that share a common schema and global catalog configuration. Domains within the same forest have implicit transitive trust relationships, allowing for authentication and access to resources across domains.
3 - Add a new forest: This means creating a completely independent instance of Active Directory. A forest is the highest security and administrative boundary in AD.
In this case, I will select the last option (Add a new forest), and the root domain name will be 'jss.com'. If there is a website called 'jmfsoft.com', it is good practice not to use that domain name as the root domain name to avoid DNS resolution conflicts.


<img width="862" height="537" alt="image" src="https://github.com/user-attachments/assets/d6f1b34f-f0c3-41a6-b17d-3c87f1d3e2c9" />











Forest functional level and domain functional level refer to the compatibility settings and features available in an Active Directory environment. These levels determine which Active Directory functions and features can be used, based on the versions of Windows Server running on the domain controllers in the environment.

1 - The "Domain functional level" specifies the features and functionalities available within a specific domain in Active Directory.

2 - The "Forest functional level" specifies the features and functionalities available across the entire Active Directory Forest, which includes all domains within that forest.

By raising the functional levels of the domain or forest, all domain controllers must be running at least the minimum version of Windows Server required by the new functional level. Once the functional level of the forest or domain is raised, it cannot be reverted to a previous functional level without restoring from a backup.

Next, there is a section called 'Specify Domain Controller Capabilities,' which has three options:

1 - Domain Name System (DNS) Server: This option allows the Domain Controller to provide name resolution services that are essential for Active Directory functionality. DNS is critical for clients and servers to locate resources within the domain. AD depends on DNS to locate domain controllers, replication services, and other critical resources. SRV (Service Location) records in DNS enable clients to locate services such as domain controllers.

2 - Global Catalog (GC): This option indicates that the domain controller contains a partial copy of all objects in the Active Directory Forest. Domain controllers acting as GCs store key information and allow quick searches across the entire forest. During logon, the GC can provide information about universal group membership, which is crucial for user authentication and authorization.

3 - Read-Only Domain Controller (RODC): This is a special type of domain controller designed for environments where the physical security of the server cannot be guaranteed, such as branch offices or remote locations. The RODC stores a read-only copy of the Active Directory database. It cannot make changes to AD, which helps protect the integrity of the directory if the server is physically compromised. By default, the RODC does not store user credentials (passwords) unless explicitly configured to do so.

Finally, we need to specify the Directory Services Restore Mode (DSRM) password, which is an important security measure in Active Directory domain controllers. It is used in directory recovery situations when the system is in a degraded state or when critical maintenance tasks need to be performed. The DSRM password allows logging in to a domain controller even if the operating system cannot start normally or if the Active Directory service is inaccessible. This provides emergency access to the domain controller in critical situations.


<img width="930" height="622" alt="image" src="https://github.com/user-attachments/assets/a77ed195-239c-468c-b4c1-a234eb4060dd" />












Next, we must verify the NETBIOS name, which is a network identifier used in Microsoft operating systems to identify resources on a local network. In the context of Active Directory, the NetBIOS name is primarily used for backward compatibility and to enable communication with legacy systems that use this technology.


<img width="926" height="611" alt="image" src="https://github.com/user-attachments/assets/93e2dc53-5de2-427a-b927-f086ee24bc7d" />













Next, the paths will be displayed, indicating the location of the folder containing the database, the folder containing the log files, and the SYSVOL folder.
1 - The database folder (C:\Windows\NTDS by default) stores the Active Directory database, where all directory objects and attributes are stored, including users, groups, policies, and system configurations. The NTDS.DIT file is the primary Active Directory database file. It contains all directory data, such as users, groups, computer objects, etc. Active Directory domain controllers (DCs) access this database to provide directory services.
2 - The log files folder (C:\Windows\NTDS by default) contains the transaction log files that record all operations performed on the Active Directory database. Transaction log (LOG) files are sequential and are used to ensure the consistency and integrity of the Active Directory database. Each transaction performed in the database is recorded in these files before being committed and written to the database. In the event of system failure or data loss, the log files can be used to recover lost data or to restore the database to a consistent state.
3 - The SYSVOL folder (C:\Windows\SYSVOL by default) stores shared data and group policies in an Active Directory environment. SYSVOL contains files and folders that are replicated among all domain controllers in the domain. This includes user login scripts, login policies, group policies, login script files, etc. In summary, these folders are critical for the operation and integrity of an Active Directory environment. The database folder stores the AD database, the log files folder records transactions, and the SYSVOL folder stores shared data and group policies.



<img width="894" height="540" alt="image" src="https://github.com/user-attachments/assets/e0412f7a-525b-4e86-884c-a69b256ed95f" />









Then, a summary of all the selected options and configurations is displayed. This allows reviewing the choices to confirm if I want to make a last-minute change before promoting the server as a domain controller. In my case, I reviewed all the options and they are correct, so I will proceed with the installation. Additionally, I can copy the executed script which contains the commands executed by the operating system to perform all the above. This is useful if I want to run a script in Windows PowerShell and automate future installations.



<img width="884" height="594" alt="image" src="https://github.com/user-attachments/assets/d6e028bd-c05e-4bc8-9902-47f3208bcdaf" />













As a final step prior to installation (i.e., promoting the server to Domain Controller), prerequisites are checked to see if the system is suitable for promotion. In this case, although it threw some usual warnings, the system is suitable to be promoted to a domain controller. After this, I click on Install and wait for the installation to complete. When the promotion operation is finished, the system will automatically restart.


<img width="880" height="565" alt="image" src="https://github.com/user-attachments/assets/a6abb125-b228-491a-9ca0-e90b2f2c72dd" />











After the server restarts, the login screen changes. Below the user and password, a message appears saying 'Sign in to JSSDOMAIN.COM', indicating that, unless specified otherwise, I'll sign in to the domain with that name. This doesn't necessarily mean that the server is a domain controller, as the login process on client machines will be the same, but it does indicate that the server is now part of the JSSDOMAIN.COM domain.
After promoting the server to a domain controller, new sections will appear in the Server Manager that didn't exist before. For example, on the left side, you'll see AD DS (Active Directory Domain Services) and DNS (Domain Name System). Then, in the tools section, several new tools will appear, such as Active Directory Administrative Center (ADAC), DNS, Active Directory Domains and Trusts, Active Directory Sites and Services, Active Directory Users and Computers (ADUC), among others.
5. Join all client machines to the domain	
Configure network settings: Ensure that all client machines and the second server can resolve the domain by setting the DNS server to the IP address of DC1.

To join client computers (and also the secondary server) to the newly created domain (JSSDOMAIN.COM), the first thing I need to do is change the DNS server to the IP address of the domain controller. This is necessary for several reasons:

1 - Name Resolution: Active Directory relies on the DNS service to resolve domain names, such as the domain name you are joining. The DNS server configured on the client needs to correctly resolve these names to locate the domain and complete the join process.

2 - Domain Controller Location: The client needs to locate the Domain Controller to join the domain and obtain configuration information. The IP address of the Domain Controller is used to identify where the Active Directory infrastructure is located on the network.

3 - Resource Records in DNS: During the domain join process, the client needs to look up specific resource records (e.g., SRV records) in DNS to locate Active Directory services, such as authentication and replication services. These records are associated with the domain name and are resolved via the DNS server configured on the client.

4 - Communication with the Domain Controller: After joining the domain, the client must be able to communicate with the Domain Controller to authenticate users, obtain group policies, and perform other Active Directory operations. Configuring the DNS server is essential for the client to correctly communicate with the Domain Controller.

If I do not point the DNS server to the IP address of the server, I get the following error:
<img width="1048" height="573" alt="image" src="https://github.com/user-attachments/assets/78fff73b-5b1b-4456-93d3-278854045d8e" />

So, to solve this issue and allow the client machines (and the additional server) to join the “JSSDOMAIN.COM” domain, I go to the network settings and set the DNS server to the IP of the domain controller, which in this case is 192.168.0.5. Although in some parts of the project the network is shown as both 192.168.0.0/24 and 192.168.0.0/24, this is because I am doing it in two geographically distant locations with two different ISPs, so this number will vary at times, but the concept remains the same.

Therefore, to do this, I go to the wireless connection icon -> Network and Sharing Center -> Change adapter settings -> Select the network adapter and right-click on it -> Properties -> Internet Protocol Version 4 -> Properties and then set the DNS server address to match the domain controller's IP address

IPv4 config OF DC01 is-------
 <img width="1015" height="577" alt="image" src="https://github.com/user-attachments/assets/c98c0a99-4499-4522-9ab5-eabfcc062c27" />


I Putted this IP at the DNS section of Client pc. Leave the section with Obtain the IP automatically.

<img width="582" height="619" alt="image" src="https://github.com/user-attachments/assets/75ac0c4a-1d15-4c9f-a529-6b7a88893f2f" />



<img width="600" height="500" alt="image" src="https://github.com/user-attachments/assets/568dd8ad-ae23-4dcd-83da-151bcb725a0f" />























Once the network configurations are completed, I need to enter credentials. The user account I need to enter must have at least 'Add computer to the domain' permissions. This permission can be granted by a Domain Admin.



<img width="934" height="704" alt="image" src="https://github.com/user-attachments/assets/e6c4cb38-68b4-4ec8-a06e-5b2f99da706f" />
















Once the credentials are entered, a message will appear indicating that the computer has been successfully joined to the domain.


<img width="497" height="325" alt="image" src="https://github.com/user-attachments/assets/033aefaf-a411-46fe-92dd-4db943cec835" />

<img width="692" height="764" alt="image" src="https://github.com/user-attachments/assets/82d3b435-d330-4357-926d-ca34883a8bd8" />

<img width="1073" height="403" alt="image" src="https://github.com/user-attachments/assets/f3df9145-63c4-4165-bba9-92cbd5dc322c" />




 

 


Now, at the next login, the same message that appeared on the Domain Controller will appear: 'Sign in to Jssdomain.com:


<img width="806" height="434" alt="image" src="https://github.com/user-attachments/assets/bf086f5b-68fc-4b13-a989-f6421f09e59e" />









6. Create Organizational Units (OUs) in Active Directory for each department	
6.1	Open Active Directory Users and Computers (ADUC) on DC1: Go to Server Manager > Tools > Active Directory Users and Computers (ADUC) or Active Directory Administrative Center (ADAC).

1 - ADAC (Active Directory Administrative Center):

Modern User Interface: ADAC has a more modern and intuitive user interface, based on PowerShell and Windows Presentation Foundation (WPF) technologies. It was introduced with Windows Server 2008 R2.

Advanced Features: It offers advanced features such as the Active Directory Recycle Bin, which allows the recovery of deleted objects without having to restore the entire system. It enables more easily configurable Role-Based Access Control (RBAC). It incorporates PowerShell Search and command history, which facilitates task automation.

Improved Security: It supports smart card authentication and other enhanced authentication methods.

Simplified Management: It provides a consolidated and simplified view to manage Active Directory objects, such as users, groups, organizational units, and others.

<img width="1016" height="504" alt="image" src="https://github.com/user-attachments/assets/7f4ce6d8-e952-4863-8609-4e0286904f1e" />



2 - ADUC (Active Directory Users and Computers):

<img width="962" height="438" alt="image" src="https://github.com/user-attachments/assets/b2f304bb-27bb-46d9-8523-107f5b88a3fc" />


Classic User Interface: ADUC has a more traditional user interface, based on the Microsoft Management Console (MMC) snap-in. It is an older tool compared to ADAC and has been used since Windows 2000.

Basic Features: It allows basic management of users, groups, computers, and organizational units in Active Directory. It does not have direct access to some of the more advanced features available in ADAC, such as the AD Recycle Bin.

Common Use in Classic Scenarios: Although it is more basic, it is still widely used for daily administration tasks due to its familiarity and simplicity.

Compatibility: It is compatible with earlier versions of Windows Server and remains a reliable tool for direct management of objects in Active Directory.

6.2	Create OUs: Right-click on the domain (e.g., JSSDOMAIN.COM) and select New > Organizational Unit. Create OUs for each department (e.g., IT, Finance, Sales, HR, Marketing, Development, Customer Service, Design, Administration).

To create organizational units (OUs), I have two options: do it from Active Directory Users and Computers (ADUC) or from Active Directory Administrative Center (ADAC):

1 - Create from Active Directory Users and Computers (ADUC): To do this, I go to Server Manager, Tools, and select Active Directory Users and Computers. Then, once inside ADUC, I expand the domain (JSSDOMAIN.COM) and select where I want to create the organizational unit.

 <img width="1015" height="543" alt="image" src="https://github.com/user-attachments/assets/ee7796af-fdf0-497a-8c9f-7f3c4681cbdc" />



It is important to note that organizational units (OUs) cannot be created within containers, nor can group policies (GP) be applied to them. In this case, I will create an organizational unit called 'Argentina' within the domain and then create within it all the OUs related to the departments of the company at the Argentina site (assuming it is a company with many locations):

<img width="1015" height="461" alt="image" src="https://github.com/user-attachments/assets/7cc266e7-3018-484d-9d43-f704e484137f" />




2 - Create from Active Directory Administrative Center (ADAC): To do this, I go to Server Manager, Tools, and select Active Directory Administrative Center. Then, once inside, I select the location, right-click, New, and select 'Organizational Unit'.

 <img width="1015" height="600" alt="image" src="https://github.com/user-attachments/assets/dd13747c-a7fa-4aac-9d23-32b010546925" />





Then, I complete the creation of the company's organizational units (IT, HR, Finance, etc.).
<img width="1014" height="439" alt="image" src="https://github.com/user-attachments/assets/3fd14965-60d5-4b8f-ad71-79a0448f5a25" />

Move user accounts to OUs: After creating user accounts, move each account to the appropriate OU by right-clicking on the user, selecting Move, and choosing the appropriate OU.

Although user creation is part of the next section, I will create a test user and move it to a previously created organizational unit (in this case, IT) just to complete point 6.3 and finish with section 1 (Homelab Creation).

<img width="1016" height="357" alt="image" src="https://github.com/user-attachments/assets/04b5ed87-fb54-48ae-be7c-df26eefd8d74" />


In this case, I will create the user named 'PAWAN' in Customer Service and then move it to the IT Organizational Unit.

<img width="770" height="324" alt="image" src="https://github.com/user-attachments/assets/d7697797-5994-4fae-8606-a5ad5835cf98" />


<img width="1050" height="366" alt="image" src="https://github.com/user-attachments/assets/8745bc25-ee46-4ea1-b6e4-5826ba53825d" />



	User and Group Creation
User Accounts and Domain Access
Although we are creating only four virtual machines to simulate end-user workstations, we will create 20 distinct user accounts within Active Directory. This approach avoids the unnecessary complexity and resource consumption of creating 20 separate machines, while still allowing us to manage and test all 20 user accounts effectively. Each user will be able to log into any of the four client machines and access the domain as if they were on a unique machine.
User List
1.	Juan Martín Franco
o	Username: juanma
o	Occupation: IT Support
o	Email: juanmafranco@jssdomain.com
o	Phone: 2325 65 1813
o	Department: IT
o	Location: Argentina
2.	Bob Smith
o	Username: bsmith
o	Occupation: Sysadmin
o	Email: bob.smith@jssdomain.com
o	Phone: (555) 123-4562
o	Department: IT
o	Location: Sant Martin
3.	Carol Davis
o	Username: cdavis
o	Occupation: Finance Manager
o	Email: carol.davis@jssdomain.com
o	Phone: (555) 123-4563
o	Department: Finance
o	Location: Chicago
4.	David Brown
o	Username: dbrown
o	Occupation: Sales Representative
o	Email: david.brown@jssdomain.com
o	Phone: (555) 123-4564
o	Department: Sales
o	Location: Miami
5.	Eve Miller
o	Username: emiller
o	Occupation: HR Manager
o	Email: eve.miller@jssdomain.com
o	Phone: (555) 123-4565
o	Department: HR
o	Location: Los Angeles
6.	Frank Wilson
o	Username: fwilson
o	Occupation: IT Support
o	Email: frank.wilson@jssdomain.com
o	Phone: (555) 123-4566
o	Department: IT
o	Location: New York
7.	Grace Moore
o	Username: gmoore
o	Occupation: Marketing Specialist
o	Email: grace.moore@jssdomain.com
o	Phone: (555) 123-4567
o	Department: Marketing
o	Location: Boston
8.	Hank Taylor
o	Username: htaylor
o	Occupation: Developer
o	Email: hank.taylor@jssdomain.com
o	Phone: (555) 123-4568
o	Department: Development
o	Location: San Francisco
9.	Ivy Anderson
o	Username: ianderson
o	Occupation: Product Manager
o	Email: ivy.anderson@jssdomain.com
o	Phone: (555) 123-4569
o	Department: Product
o	Location: Seattle
10.	Jack Thomas
o	Username: jthomas
o	Occupation: Sysadmin
o	Email: jack.thomas@jssdomain.com
o	Phone: (555) 123-4570
o	Department: IT
o	Location: San Francisco
11.	Kathy White
o	Username: kwhite
o	Occupation: Customer Service Representative
o	Email: kathy.white@jssdomain.com
o	Phone: (555) 123-4571
o	Department: Customer Service
o	Location: Houston
12.	Leo Harris
o	Username: lharris
o	Occupation: Network Engineer
o	Email: leo.harris@jssdomain.com
o	Phone: (555) 123-4572
o	Department: IT
o	Location: New York
13.	Mona Martin
o	Username: mmartin
o	Occupation: Content Writer
o	Email: mona.martin@jssdomain.com
o	Phone: (555) 123-4573
o	Department: Marketing
o	Location: Boston
14.	Nate Jackson
o	Username: njackson
o	Occupation: Database Administrator
o	Email: nate.jackson@jssdomain.com
o	Phone: (555) 123-4574
o	Department: IT
o	Location: Chicago
15.	Lara Vega
o	Username: laravega
o	Occupation: Graphic Designer
o	Email: laravega@jssdomain.com
o	Phone: (555) 123-4575
o	Department: Design
o	Location: Argentina
16.	Paul King
o	Username: pking
o	Occupation: IT Support
o	Email: paul.king@jssdomain.com
o	Phone: (555) 123-4576
o	Department: IT
o	Location: New York
17.	Quinn Scott
o	Username: qscott
o	Occupation: Sales Representative
o	Email: quinn.scott@jssdomain.com
o	Phone: (555) 123-4577
o	Department: Sales
o	Location: Miami
18.	Rachel Adams
o	Username: radams
o	Occupation: Finance Analyst
o	Email: rachel.adams@jssdomain.com
o	Phone: (555) 123-4578
o	Department: Finance
o	Location: Chicago
19.	Sam Turner
o	Username: sturner
o	Occupation: DevOps Engineer
o	Email: sam.turner@jssdomain.com
o	Phone: (555) 123-4579
o	Department: IT
o	Location: Seattle
20.	Tina Phillips
o	Username: tphillips
o	Occupation: Office Manager
o	Email: tina.phillips@jssdomain.com
o	Phone: (555) 123-4580
o	Department: Administration
o	Location: Los Angeles

Given the list of users, I will proceed to create 10 users with ADAC and 10 users with ADUC to practice both 'programs'. Accidental deletion protection will be activated in all cases.

To simulate a 'real' work environment, I'm going to create a ticket in Jira Service Management (JSM) representing a hypothetical request for user creation.

<img width="1096" height="738" alt="image" src="https://github.com/user-attachments/assets/de137b38-dd5c-4680-b70c-d83db73730fc" />






I invited me through e-mail to join this project and assign tickets to myself.

<img width="745" height="481" alt="image" src="https://github.com/user-attachments/assets/becde3f3-425b-4e04-b497-4505071b1d50" />



<img width="704" height="646" alt="image" src="https://github.com/user-attachments/assets/8521cb29-e8c5-4f4c-8051-bab27f25deba" />



























Now, I will create the ticket that simulates the experience of receiving it, 'working on it', and resolving it.

<img width="1042" height="586" alt="image" src="https://github.com/user-attachments/assets/4707da31-f4dc-46e7-bec1-8f27b6bcd299" />


After simulating the ticket sent by Rahul Das (a user created by myself), I assign it to myself (indicating that I will handle the ticket) and start creating the users.
<img width="1089" height="557" alt="image" src="https://github.com/user-attachments/assets/8683c667-871d-42f0-9d78-389508a7a0bf" />

Finally, once the ticket is resolved (with all the users created), I get the following:
<img width="1015" height="551" alt="image" src="https://github.com/user-attachments/assets/b04f4898-185a-42f8-8567-4a8623ec3c91" />




Therefore, once I have created all the requested users, I can mark the ticket as resolved and additionally leave a message for the user who initiated it, indicating that all the users have been successfully created.

<img width="1016" height="465" alt="image" src="https://github.com/user-attachments/assets/a249a2ea-38c9-4e1d-bc47-b4b8746ab5a8" />



•	User creation through Azure Active Directory (currently Microsoft Entra ID)
In addition to managing Active Directory on-premises, you can use the cloud version, which is Azure Active Directory, currently known as Microsoft Entra ID.
<img width="1016" height="429" alt="image" src="https://github.com/user-attachments/assets/7cab5f67-ee04-4fbe-9f9f-c53f5df7cc5e" />

To do this, I must first have my domain created (in this case, I created the domain Jobskillshare408.onmicrosoft.com).
To create users, I go to the Users section and select New User -> Create New User:
<img width="1016" height="531" alt="image" src="https://github.com/user-attachments/assets/4d594de9-35fa-4ea6-bdc8-85d1e1c4aa91" />


Click Create a new user.

<img width="905" height="427" alt="image" src="https://github.com/user-attachments/assets/413b5423-c105-4a88-a32e-5d0985e502cb" />


<img width="923" height="460" alt="image" src="https://github.com/user-attachments/assets/4c84e605-fe6a-43f9-806c-997d9890fd5c" />

















Finally, I check that all the fields have been filled in correctly:


<img width="937" height="455" alt="image" src="https://github.com/user-attachments/assets/b51ecdf5-eaca-48fe-88b4-efa3dce4f736" />









If everything is correct, I press 'Create' to finish creating the user:
<img width="1016" height="327" alt="image" src="https://github.com/user-attachments/assets/87d605e1-e2fa-4cee-b6ea-b36130429b62" />

	Bulk user creation with Azure Active Directory using a .csv file
To create multiple users at once, Azure Active Directory has a section that allows you to create multiple users based on an uploaded .csv file.
To do this, I go to Bulk operations -> Bulk Creation and download the corresponding .csv.
<img width="1016" height="373" alt="image" src="https://github.com/user-attachments/assets/ec975750-c464-4973-a2d5-b015ef15d041" />

Then, I downloaded the sample csv file to create bulk user.
<img width="1055" height="387" alt="image" src="https://github.com/user-attachments/assets/145a6655-ef03-431b-8c45-22f25590ebbd" />

And this is the format.
<img width="1078" height="196" alt="image" src="https://github.com/user-attachments/assets/8e100023-dae8-45cc-a6d9-229940f937b5" />

Then I put the data of employees from the ticket. After that I Submitted the Csv file.
<img width="1015" height="543" alt="image" src="https://github.com/user-attachments/assets/0cd07b4f-6d8e-4a28-ba7b-be6245f06b6c" />


Once the .csv is downloaded, I fill in each column with the corresponding data, where each row will correspond to a particular user.
Once the users are created, I get the following:
<img width="814" height="362" alt="image" src="https://github.com/user-attachments/assets/2d7e3112-e3ce-4ea6-9ee8-5a53342bd754" />

	Microsoft 365 User Creation
Steps to follow to create users in Microsoft 365:
First, I go to the Microsoft 365 Administration Center.
Then, on the left panel, go to the Active Users section:
<img width="1015" height="424" alt="image" src="https://github.com/user-attachments/assets/90acd844-0f15-4b13-b0f4-0a8c024ddb1f" />

Then, I fill in the fields, indicating first name, last name, display name (which will appear in the list of active users) and the user name, with which the user will log in:
<img width="1009" height="625" alt="image" src="https://github.com/user-attachments/assets/3579f286-14b4-42c7-ac1b-577fa29d04d9" />














Here I have to assign a license to the user, because I don’t have any license so I clicked create user without product license.

<img width="617" height="366" alt="image" src="https://github.com/user-attachments/assets/6265b22a-7659-4267-aa40-4bf9e454b2a5" />







Then, in the optional configuration section, you configure whether you want the user to be a "normal" user or an administrator user.
If so, you will choose what type of administrator user it is, such as: Helpdesk Administrator, Exchange Administrator, Teams Administrator or Global Administrator if you want to give unlimited access to all features:


<img width="618" height="408" alt="image" src="https://github.com/user-attachments/assets/986cb520-e3ce-4488-9fae-9a943d0f55b4" />







Finally, it remains to confirm that all the data is correct and finish with the user creation.



<img width="614" height="433" alt="image" src="https://github.com/user-attachments/assets/a3419cca-9ad9-433c-bb8e-e5406dc47272" />



<img width="777" height="441" alt="image" src="https://github.com/user-attachments/assets/303ddec5-eb98-48d2-b4e8-a6e0a82eaaa9" />














Microsoft 365 Group Creation
Before going straight to the creation of groups, it is a good idea to define the different types of groups available in Microsoft 365 and the characteristics of each one.
Microsoft 365 Group: A collaborative group that allows shared access to resources like Outlook, Teams, SharePoint, and more. It is fully compatible with all Microsoft 365 applications, is assigned an email address, and can integrate with Azure Active Directory for security management.
Distribution List: Used to send emails to multiple recipients simultaneously. It is not compatible with Microsoft 365 applications and is only used for email distribution. It does not offer additional security features.
Security Group: Designed to control access to resources within Microsoft 365 through roles and permissions. It is not compatible with Microsoft 365 applications and does not have an email address assigned. It offers strict access control to resources.
Mail-Enabled Security Group: Similar to a security group but also allows sending emails to its members. It is not compatible with Microsoft 365 applications, but it is assigned an email address and offers strict access control to resources.
Once you understand the differences between each type of group, it is proper to continue with the steps to create them.
Steps to follow for the creation of groups in Microsoft 365:
First, I go to the Microsoft 365 Administration Center.
Then, on the left panel, go to the Active groups and teams’ section:
<img width="1015" height="193" alt="image" src="https://github.com/user-attachments/assets/aa14fc87-ecbb-4ba4-9fc1-05c7bfed7126" />

	Then, we fill in the following data:

Group name and description
Owner/s of the group
Group members
Email (shared) of the group
If the group is public or private
If I want to connect Microsoft Teams to the group

<img width="801" height="441" alt="image" src="https://github.com/user-attachments/assets/c1d54c1b-80b3-44cf-8bb3-74989a1fba19" />
















Now I have to quickly resolve the ticket with respect to SLA.

<img width="834" height="498" alt="image" src="https://github.com/user-attachments/assets/46331f42-9fd7-4ae7-93b2-58ec0ae62322" />


















Then I Assign owners.

<img width="598" height="348" alt="image" src="https://github.com/user-attachments/assets/72b58394-4fdc-449c-af63-c86d3f5887b8" />









Then add the members.

<img width="801" height="439" alt="image" src="https://github.com/user-attachments/assets/cc7f0073-a66b-47a9-a1f9-0c9fafd78c4f" />



<img width="802" height="425" alt="image" src="https://github.com/user-attachments/assets/b8192b41-ab97-478d-83fb-afbdec12c6d8" />




























Then I named the group and make it privet according to company policy regarding to the Graphic design department. After that I allow admin roles to be assign for this group.


<img width="822" height="434" alt="image" src="https://github.com/user-attachments/assets/05431076-29c2-4f46-a26b-9666cff2b547" />













After creating all stuffs I review and check the final view , therefore clicked on create group.


<img width="833" height="473" alt="image" src="https://github.com/user-attachments/assets/10d330c7-b97b-4060-a7d2-6e9ed342591a" />
















	Group Policy Management and              Implementation
GPOs (Group Policy Objects) are a feature of Windows Server operating systems that allow administrators to centrally manage and configure policies and settings for systems and users in an Active Directory domain network.
Basically, GPOs are collections of policy settings that can be applied to users and computers in an Active Directory-based network environment. These policies can cover a wide range of settings, from system security to user environment customization.
<img width="811" height="511" alt="image" src="https://github.com/user-attachments/assets/d344bd47-57f7-492d-88a3-2f9f0a407e80" />

There are some basic concepts of GPOs that need to be understood before starting to configure them:
GPO (Group Policy Objects) linking
GPOs must be linked to a container in Active Directory to be applied.
Containers can be sites, domains or Organizational Units (OUs).
<img width="1016" height="742" alt="image" src="https://github.com/user-attachments/assets/729f2449-eaa1-43c0-aa2e-4330aa3b6eba" />


GPO Inheritance / Precedence
GPO inheritance refers to how policies configured in GPOs are applied and inherited through the Active Directory hierarchy.
The hierarchy follows this order:
1.	Local: Local policies on the computer.
2.	Site: Policies linked to a site in Active Directory.
3.	Domain: Policies linked to the domain.
4.	Organizational Units (OUs): Policies linked to specific OUs.
Order of Application:
1.	Local: Local policies are applied first.
2.	Site: Site policies are applied after.
3.	Domain: Domain policies are applied next.
4.	OUs: OU policies are applied in order from the top-level OU to the most specific OU containing the object (user or equipment).
<img width="1016" height="500" alt="image" src="https://github.com/user-attachments/assets/13c3e693-3d6a-46ce-9a0d-7f1aeda15ab0" />

Inheritance Modifiers
Mechanisms exist to modify how GPOs are inherited.
1.	Inheritance Blocking:
You can block inheritance of GPOs into a specific OU. This means that higher level policies will not be applied to that OU.
How to enable: In the GPMC, select the OU, right click and select "Block Inheritance".
2.	Enforced:
A GPO can be marked as "Enforced", which means that its policies cannot be blocked by any lower OU.
How to enable: In the GPMC, select the GPO, right click and select "Enforced".
3.	Security Filtering:
GPOs can be filtered to only apply to specific users or computers by using Security Groups.
How to configure: In the GPMC, select the GPO, go to the "Scope" tab and adjust the permissions in the Security Filtering section.
Now that all the basic concepts are understood, it is time to show the steps for the application of such group policies.
Steps to configure and apply a GPO
1.	Open the Group Policy Management Console (GPMC):
Log on to a domain controller or machine with the administrative tools installed (RSAT).
Open the Group Policy Management Console (GPMC). You can do this by searching for "gpmc.msc" in the Start menu or through Administrative Tools.

<img width="920" height="477" alt="image" src="https://github.com/user-attachments/assets/d6e74fe3-7fd6-413c-98b1-82299d510a45" />

<img width="1096" height="589" alt="image" src="https://github.com/user-attachments/assets/c3fc0783-d841-465c-a401-eec312b7ce0d" />



















	Create a New GPO:

In the GPMC, navigate to the container in which you want to create the GPO. This can be a domain, site, or an organizational unit (OU).
Right-click on the container and select "Create a GPO in this domain and link it here...".

In this case, I will create a GPO that applies to the entire domain.
<img width="1016" height="516" alt="image" src="https://github.com/user-attachments/assets/6b0219ea-5516-4654-b486-fd5d514d6e90" />


	Name the GPO:

Assign a descriptive name to the new GPO. For example, "Password Policy" or "Desktop Wallpaper".

In this case, I will configure the GPO to apply a secure password policy.

<img width="946" height="505" alt="image" src="https://github.com/user-attachments/assets/7140dd9f-915f-49e3-b4e7-e4cf576fd5cf" />



<img width="1016" height="160" alt="image" src="https://github.com/user-attachments/assets/92af407d-2c75-4c0d-8cab-495d3347b05b" />
















	Edit the GPO:

Once created, the new GPO will appear in the list of GPOs linked to the selected domain, site or OU.

Right-click on the GPO and select "Edit" to open the Group Policy Management Editor.

<img width="900" height="444" alt="image" src="https://github.com/user-attachments/assets/1ce64392-bb80-48db-b996-26c26aa37db7" />

















Once this is done, the Group Policy Management Editor will open.

	Configure Desired Policies:

In the Group Policy Management Editor, you can configure policies under two main categories:
Computer Settings:
These policies are applied at the computer level. Examples include security settings, software installation, network policies, etc.
User Configuration:
These policies are applied at the user level. Examples include desktop settings, folder redirection, software restrictions, etc.
<img width="1015" height="452" alt="image" src="https://github.com/user-attachments/assets/b4111138-75d9-4cf5-9790-61662e215b87" />

In this case, I go to:
Computer Configuration >policies> Windows Settings > Security Settings > Account Policies > Password Policy.
<img width="1093" height="493" alt="image" src="https://github.com/user-attachments/assets/4e3313bb-9180-4a09-923b-eb80dc7120b2" />


Now, I double click on the policy to be modified and a window will open that will allow me to enable/disable this policy and also to modify the values.

<img width="747" height="743" alt="image" src="https://github.com/user-attachments/assets/d1b46d70-a99b-4bf5-847a-01d57ffb59ce" />
























Finally, I click on "Apply" and the password length policy is set to 8 characters long.
<img width="1088" height="239" alt="image" src="https://github.com/user-attachments/assets/fd1ce041-68be-4160-ab5f-ec98d33adc24" />


	Apply the GPO:

The GPO is already bound to the selected domain, site or OU, and the policies will be automatically applied to objects in that scope.
The GPO will be processed on the next policy update cycle (typically every 90 minutes on computers and at user login).
Force Policy Update:
To enforce policies immediately, you can force an update on affected computers and users:
On the Domain Controller:
Run gpupdate /force at the command prompt to update policies on the domain controller.
On Client Computers:
On each computer, run gpupdate /force at the command prompt to apply the new policies immediately.
<img width="1016" height="215" alt="image" src="https://github.com/user-attachments/assets/03483289-c7d2-4ba6-9982-ca0966f4668d" />

Other examples of group policies that from my point of view would be good to apply
In addition to the password group policy that strengthens security, there are others that I believe are essential to implement in a business environment:
1.	Disable the use of USB devices:
Policy: Deny read and write access to removable storage devices.
Benefit: Mitigates the risk of data loss and the introduction of malware through unauthorized USB devices.
Location: Computer Configuration > Policies > Administrative Templates > System > Removable Storage Access
<img width="1085" height="521" alt="image" src="https://github.com/user-attachments/assets/bd1993e2-3bf2-4756-8094-5e29c43ee903" />




	Disable the installation of unauthorized software:
Policy: Disable installation of devices matching any of these device IDs.
Benefit: Ensures that only approved software is installed, reducing the risk of system vulnerabilities and conflicts.
Location: Computer Configuration > Policies > Administrative Templates > System > Driver Installation
<img width="1015" height="613" alt="image" src="https://github.com/user-attachments/assets/171ca959-1418-41f2-9c2c-699f7d3b15bf" />


	Disable access to the Control Panel and Settings:
Policy: Prohibit access to the Control Panel and Settings.
Benefit: Prevents unauthorized changes to system settings and reduces the technical support burden due to inadvertent configuration.
Location: User Configuration > Policies > Administrative Templates > Control Panel
<img width="1108" height="358" alt="image" src="https://github.com/user-attachments/assets/aa496b3e-73b0-4d40-b8c8-5f1f255aef4c" />


 	Example of what happens if I try to perform an action blocked by a group policy
If I try, for example, to access the control panel after it has been locked by group policy and group policies have been updated (either by their natural cycle or by the gpupdate /force command), I get the following message:


<img width="853" height="722" alt="image" src="https://github.com/user-attachments/assets/674bf798-0dda-447a-b206-0f309e94e6e6" />
























	Password Reset

Now, to continue with the Homelab, I will practice another fundamental skill, which is resetting user passwords. Again, I will create a ticket from the same person to simulate a real environment.
Once the ticket is created and the request is submitted, I log in as 'IT Support' and view the ticket in question:

<img width="1016" height="480" alt="image" src="https://github.com/user-attachments/assets/bfb179eb-fadf-4242-8a2b-c35013068d0d" />

To do this, I go to Active Directory Users and Computers (ADUC) or Active Directory Administration Center (ADAC), and locate the user.
•	Reset password on Active Directory Users and Computers (ADUC)
In this case, the search can be done through the organizational units (as marked with 1, which gave me the results of the design department) or through the search function, useful when we have many users (as marked with 2):


<img width="880" height="507" alt="image" src="https://github.com/user-attachments/assets/4e33c1f1-2cbc-4568-ac95-7909271cab88" />
















Once I have located the user, I right click on it and 'Reset password':


<img width="648" height="687" alt="image" src="https://github.com/user-attachments/assets/95854bee-eb95-4673-ae4b-ec27b73b56b0" />























Now, I must enter a temporary password that will be given to the user to try to log in next time.
It is important to check the box that indicates that the user will have to change the password at the next login, this is a good practice and its use is recommended.
Finally, it gives us the possibility to unblock the user, although in this case the user did not actually block the account.


<img width="639" height="555" alt="image" src="https://github.com/user-attachments/assets/dba75efc-2db5-49da-a5c1-f2225ddc11e7" />

















Once this is done, I get a confirmation message:


<img width="525" height="281" alt="image" src="https://github.com/user-attachments/assets/04b2073d-7fbb-4053-8514-1a694d713419" />










•	Reset password on Active Directory Administrative Center (ADAC)
Resetting a password in the Active Directory Administrative Center is just as easy, in addition to the fact that it has a more user-friendly interface.
Once ADAC is open, we can locate a section where we can easily reset a user's password:
<img width="1016" height="305" alt="image" src="https://github.com/user-attachments/assets/04882f7a-5931-4547-939d-44d7e49fc406" />

Now, once I have located the user, I can choose to right click on it and select 'Reset password' (1) or simply use the right sidebar (2) which also has this option.
<img width="1016" height="227" alt="image" src="https://github.com/user-attachments/assets/20b6be32-4321-4702-a8dd-ee01b8e817f4" />

Again, I fill in the field with the temporary password and select the option for the user to change it at the next login:
<img width="1015" height="214" alt="image" src="https://github.com/user-attachments/assets/f8e2e949-756e-42ff-925c-b5ee99cf72ce" />

Done! Once this is completed, the user must log in with the temporary password and then will be prompted to update it.
<img width="1096" height="559" alt="image" src="https://github.com/user-attachments/assets/09ffe0e6-0103-4199-81d8-08be400241e1" />




•	Reset password on Azure Active Directory
To reset a password in Azure Active Directory, I go to the users section.
<img width="1030" height="453" alt="image" src="https://github.com/user-attachments/assets/1bc35bc5-9945-482e-a1eb-3bc7580eb494" />





Then, I select the user to reset the password.
In this case, as the ticket indicated, the user is “Lara Vega”:

<img width="1016" height="501" alt="image" src="https://github.com/user-attachments/assets/b6c43260-db5a-442a-9fa9-70b053b9c2b7" />



<img width="486" height="318" alt="image" src="https://github.com/user-attachments/assets/5d5f0f76-d3db-4520-95e7-f29ee11edba1" />









Once the button is pressed, the user's password will be reset and a temporary password will be automatically generated, which must be given to the user so that he/she can change it later.
In this case, the temporary password is “Tanu2149”.

<img width="851" height="418" alt="image" src="https://github.com/user-attachments/assets/85cec175-abb1-4318-8765-078ca736cc4e" />













	Multi-factor authentication (MFA)

Multi-Factor Authentication (MFA) is a crucial security measure that adds an additional layer of protection to digital systems and accounts. Its main purpose is to mitigate the risks associated with password-only authentication, which can be vulnerable to various forms of attacks such as phishing, password theft, and the use of weak passwords.
<img width="1015" height="481" alt="image" src="https://github.com/user-attachments/assets/e6029800-0f01-4ee3-ae18-482e9c3de396" />


Enabling multi-factor authentication in an enterprise environment is a best practice because it significantly enhances security, reduces the risk of account compromise and meets regulatory standards, all while improving access management and protecting the organization's critical data from both internal and external threats.
First, I am going to simulate the creation of a ticket requesting the activation of this feature (Multifactor Authentication).
<img width="1064" height="527" alt="image" src="https://github.com/user-attachments/assets/ef854625-f5a9-46c5-b4d8-4841a7b71844" />

To do this, I perform the following steps:
1.	First, I go to the Azure portal --> https://portal.azure.com/
<img width="1016" height="453" alt="image" src="https://github.com/user-attachments/assets/ac2a205e-4be7-482e-b5df-c01542b562e2" />
<img width="1015" height="494" alt="image" src="https://github.com/user-attachments/assets/a8b41931-fc6b-4cad-b44a-2a5984e91644" />

 
Then I Enable the pop up.

<img width="561" height="361" alt="image" src="https://github.com/user-attachments/assets/7a72a368-8ed9-4aba-b7ef-950eccd407eb" />











Once accepted, the following banner is displayed, indicating that multi-factor authentication has been enabled for the selected users:

Finally, it remains to answer the ticket indicating to the user (in this case, Carlos Perez), that the multifactor authentication (MFA) was successfully enabled.
In addition, evidence is attached to the ticket demonstrating the indicated action.
 <img width="1015" height="672" alt="image" src="https://github.com/user-attachments/assets/03945df4-a200-4575-b694-4e7270564d73" />

	Software Deployment Through PDQ Inventory
In this section, I will use a software deployment program, PDQ Deploy, to simplify and automate the installation and updating of applications across multiple machines in my Homelab. The reasons for implementing a software deployment tool like PDQ Deploy are:
•	Efficiency and Time-Saving: Manually installing or updating software on each client machine can be very time-consuming, especially in a larger environment. PDQ Deploy streamlines this process by allowing for the automated deployment of software packages across all connected devices, saving significant time and effort.
•	Consistency and Control: Using PDQ Deploy ensures that all machines receive the same software version and configuration, reducing the risk of discrepancies and compatibility issues. This helps maintain a uniform and controlled IT environment.
•	Centralized Management: PDQ Deploy provides a centralized interface to manage software installations, making it easier to track deployment status, manage software versions, and perform updates from a single point of control.
•	Scalability: As my Homelab grows, manually handling software installations becomes increasingly impractical. PDQ Deploy scales with the environment, allowing for efficient software management regardless of the number of machines involved.
In this case, I will do it from the secondary server (SV02) to balance the load and not overburden the domain controller (DC01).
This tool can be downloaded from the following website: PDQ Deploy
Now, I proceed to download and install it on the secondary server (SV02).
To do this, I need to create an account, which will give me a free trial of the full program for 14 days, although the basic functionality (software deployment) can still be used after those 14 days:
<img width="1106" height="622" alt="image" src="https://github.com/user-attachments/assets/f048a9da-b92d-4915-a3b5-d56382ad54b6" />


Once registered, I am redirected to the following page where I get the links to download PDQ Deploy & PDQ Inventory, along with their licenses (which last for 14 days).

<img width="1074" height="515" alt="image" src="https://github.com/user-attachments/assets/784b03bc-1f3f-4379-993a-e3ec7e8d754d" />

After installing (as a server, since DC01 will be in charge of deploying to client machines), the program interface will look like this:
<img width="1016" height="545" alt="image" src="https://github.com/user-attachments/assets/7757b0be-d17c-4325-a103-d58a2cca7adc" />

Now, to begin the deployment, first, I need to have the installer of the software I want to deploy downloaded and saved in a location.
In this case, for simplicity, I'll choose to install 7zip.
The installer will be located in a folder on the desktop named 'Software to Deploy':
<img width="1015" height="334" alt="image" src="https://github.com/user-attachments/assets/8d3b8c99-c107-4652-9d91-49464eb73221" />

Now, I open PDQ Deploy and select 'New Package'. Then, I fill in the fields.

<img width="893" height="542" alt="image" src="https://github.com/user-attachments/assets/888da193-d3c5-4008-9d07-ca3220a303af" />



























Afterwards, I go to 'Steps' and then select 'Install':


<img width="918" height="612" alt="image" src="https://github.com/user-attachments/assets/6336cb58-3e7c-42ba-b36f-0e5a04a57622" />



























Then, I select the installer, add the parameters for silent installation (in this case, the appropriate one for 7zip is /S) to avoid disturbing any user while they are working, and press save.
<img width="977" height="621" alt="image" src="https://github.com/user-attachments/assets/cb414feb-cfe9-49f6-8865-1f1b74f71583" />

Once this is done, the package will appear already created. Now, all that's left is to right-click on it and press 'Deploy Once' to begin the deployment.

<img width="1015" height="580" alt="image" src="https://github.com/user-attachments/assets/3367d763-9def-4ff2-b311-586d06f4b5cb" />



Finally, I must select the users/computers by clicking on 'Choose Targets', and that's it, I can start the deployment:
 
<img width="1015" height="680" alt="image" src="https://github.com/user-attachments/assets/8bd8dbca-7aa1-47e9-8b5a-4b69715f5c94" />


In this case, just for testing purposes, I'll select client machines 1 & 2 (JssPC01, JssPC02).

<img width="1015" height="576" alt="image" src="https://github.com/user-attachments/assets/133a4f43-1ae5-4ba3-9443-68b3db951a06" />

<img width="1015" height="722" alt="image" src="https://github.com/user-attachments/assets/885fa1ec-7e0a-42ba-a7ea-26e8fc669a9e" />


After initiating the deployment, I get the following results (in this case  I deploy on only one Pc[JssPC01] )
<img width="1084" height="600" alt="image" src="https://github.com/user-attachments/assets/69984edc-7b46-49ed-ad07-531f316ec2c9" />

Now, I verify that 7zip was installed correctly on the client machines:
<img width="1111" height="923" alt="image" src="https://github.com/user-attachments/assets/b6c2bf4b-24fe-4d08-ac1b-a846ccef4847" />

And, with this, I can successfully conclude the deployment.















	Remote Access and Troubleshooting
In this section of the project, remote access between company computers will be implemented for troubleshooting purposes.
This concept is fundamental in day-to-day IT support, since it is a required skill that is very useful because at times it may not be possible to physically access a computer (due to geographical issues, for example).
There are many tools on the market that facilitate remote access, such as TeamViewer or AnyDesk. Even Windows comes with a tool built into the operating system itself, but for reasons of versatility I will use TeamViewer because it works on many operating systems and it is also my tool of choice, but I could easily use AnyDesk, which also has a very simple interface, less resource consumption and lower licensing costs.
<img width="312" height="312" alt="image" src="https://github.com/user-attachments/assets/259052d7-023c-4720-9b6b-9d02834f6630" />

 

In this case, the connection will be made from the SV02 machine to the JMFSOFT-PC01 client machine.
To do this, I must perform the following steps:
1.	First, I need to install TeamViewer on both computers.
To download the executable, I go to https://www.teamviewer.com/ and click on Free Download.
<img width="1042" height="595" alt="image" src="https://github.com/user-attachments/assets/f889ab34-d6dd-4b1d-87c8-563306616766" />

Once this is done, I select the desired plan.
In this case, I click on "Free", but in a business environment you may have to opt for the other licensing options.
<img width="1015" height="422" alt="image" src="https://github.com/user-attachments/assets/2ac79939-c326-45e2-a9c3-530416331ecf" />
 


Now, I must choose which type of installation I want, each one with its respective functionalities and/or advantages as appropriate.
Among the options are:


•	TeamViewer Quick Support: No full installation is required; it is a lightweight, portable application. Ideal for quick and occasional support.
•	TeamViewer Full Client: Offers all TeamViewer functionalities for full remote access and control. Allows both inbound and outbound connection, which means you can control other devices or be controlled. Includes features such as file transfer, chat, and Wake-on-LAN.
•	TeamViewer Host: Provides continuous remote access to devices, ideal for long-term remote management. Requires installation on the device. Allows unlimited and permanent remote connections. Ideal for keeping computers and servers under constant surveillance and support.
•	TeamViewer MSI Package: Facilitates the distribution and management of TeamViewer installations through group policies in corporate networks. MSI format suitable for deploying and managing installation on multiple devices via Active Directory and GPOs. Supports custom configurations to suit specific needs. Allows automating installation and configuring pre-deployment settings.
•	TeamViewer Meeting: Dedicated platform for online meetings, video conferencing, and real-time collaboration. Provides tools for high-quality audio and visual communication. Includes screen sharing, chat, and meeting recording functionalities.
Below is a comparative table showing the main features between each version:
 <img width="1015" height="594" alt="image" src="https://github.com/user-attachments/assets/0c429006-ec43-44d5-972d-c165dd4b8877" />

In this case I chose to install the "Full Client" version on the DC01, in its 64-Bits version.
Once downloaded and installed, the client is executed, which looks as follows:
<img width="1015" height="766" alt="image" src="https://github.com/user-attachments/assets/e9761180-85c3-4006-961b-c0dea055ae44" />
 

To make a connection, it is necessary to use the ID and password.

These credentials should only be shared between the IT Support Technician and the person who wishes to receive help from the IT Support Technician, otherwise anyone could gain remote access to the company's equipment.

Now, I download and install the Quick Support version on the JSS-PC01 computer (just for demonstration and simplicity, as I could have opted for the Full Client version).

<img width="656" height="801" alt="image" src="https://github.com/user-attachments/assets/d31143ad-b80c-4f58-9fad-cd64de9ecd44" />
































Finally, just share the credentials of the client computer (JMFSOFT-PC01) with the IT Support manager so that he/she can enter them in the TeamViewer client to gain remote access and troubleshoot problems.






	VPN Configuration
In addition to using remote access to troubleshoot problems, a good practice is to use remote access via VPN.
A Virtual Private Network (VPN) is a technology that creates a secure, encrypted network connection over a public network, such as the Internet. It allows users to send and receive data securely over shared or public networks as if their devices were connected directly to a private network.
Why is it considered a good practice?
•	Data Security:
o	Encryption: VPNs use encryption protocols to protect the data transmitted between the user and the VPN server. This makes the information unreadable to anyone intercepting the connection.
o	Authentication: VPNs may require authentication, which ensures that only authorized users can access the network.
•	Privacy:
o	IP Hiding: The VPN hides the user's IP address, which helps maintain online privacy by making user activity less traceable.
o	Identity Protection: By hiding the user's location and identity, VPNs protect against tracking and data collection by third parties.
•	Secure Remote Access:
o	Remote Work: VPNs allow employees to securely access the corporate network from remote locations, which is crucial for teleworking and workforce mobility.
o	Corporate Resources: Users can access internal corporate resources, such as files, applications and databases, as if they were physically in the office.
•	Censorship and Geographical Restrictions Avoidance:
o	Accessing Blocked Content: VPNs allow users to access websites and services that may be blocked or restricted in their geographic location.
o	Censorship Avoidance: They can be used to circumvent censorship in regions where Internet access is controlled or restricted.
•	Information Integrity:
o	Protection Against Tampering: VPNs' encryption and security protocols protect data from alteration and tampering while in transit.
Conclusion
Implementing VPNs is a best practice for its ability to provide a secure and private connection over public networks, protect sensitive information and allow secure remote access to network resources. This not only improves overall network security, but also ensures the integrity and privacy of transmitted data.
Steps to set up a VPN
1.	First, I must choose on which server I want to install the role.
In this case I will choose to install the Remote Access role on the SV02 server. This helps distribute the workload and improves security by not directly exposing the domain controller to external connections.
To start, I open the Server Manager:
<img width="1016" height="762" alt="image" src="https://github.com/user-attachments/assets/127aede6-1e92-4e4c-a200-fad4cd28d764" />

Then, to add the role,  I go to “Manage” and then “Add roles and features”:
<img width="1016" height="407" alt="image" src="https://github.com/user-attachments/assets/42cb7596-afa9-4c05-96be-0d4d13e02225" />

Now, I select the “Remote Access” role and install it:
<img width="1108" height="720" alt="image" src="https://github.com/user-attachments/assets/5c7ee7f3-62f8-4ed2-a78d-82f911787d6a" />

Then, I select the “DirectAccess and VPN (RAS)” option:
<img width="1016" height="669" alt="image" src="https://github.com/user-attachments/assets/7c6edd31-6bcb-4adf-82a1-9fd4ef9d3863" />

o	DirectAccess is a remote access technology introduced in Windows Server 2008 R2. It allows remote users to access internal network resources without the need to manually initiate a VPN connection. DirectAccess automatically establishes a secure, persistent connection between the user's device and the corporate network, providing an experience similar to being in the office.
o	VPN is a technology that creates a secure, encrypted connection over a less secure network, such as the Internet. VPN allows remote users to access internal network resources as if they were directly connected to the network.
2.	Finally, I review the selected items and confirm that everything is correct to start with the installation of the Remote Access role, so I press “Install”:
<img width="1016" height="646" alt="image" src="https://github.com/user-attachments/assets/586791e4-4c6e-4977-aa05-d10a8c110008" />

Once the installation is complete, a message will be displayed indicating that the installation was successful, but that configuration is required:

<img width="1038" height="705" alt="image" src="https://github.com/user-attachments/assets/db190efa-fad6-470f-a4d5-231c1203b20f" />

<img width="1067" height="699" alt="image" src="https://github.com/user-attachments/assets/bb543463-2551-447d-ab8e-e6851505555f" />







Now, it is time to configure the Virtual Private Network (VPN), which will allow remote access to employees in a distant geographical location.
To do this, I must open the “Introduction Wizard”, which is displayed when I finish installing the “Remote Access” role:
<img width="1016" height="710" alt="image" src="https://github.com/user-attachments/assets/8f7594a0-9f82-4f3f-8589-359f3aa2f6d8" />

Once the wizard is opened, a window containing 3 options will be displayed:

<img width="628" height="509" alt="image" src="https://github.com/user-attachments/assets/71fa1da7-beb3-461c-b8ca-1808a5db79c3" />









Deploy both DirectAccess and VPN (Recommended): Implementing both VPN and DirectAccess offers flexibility, allowing users to choose between automatic (DirectAccess) or manual (VPN) connections based on their needs. This option provides compatibility across various devices and operating systems that do not support DirectAccess, such as macOS and Linux. DirectAccess ensures a seamless and continuous connection to the corporate network, ideal for users who require uninterrupted access. It also enhances security by utilizing multiple authentication and encryption methods. However, managing both technologies can be complex, requiring more time and resources, and DirectAccess requires IPv6, which might necessitate additional network configurations.
Deploy DirectAccess only: Implementing DirectAccess Only provides a transparent connection experience, as users do not need to manually initiate the connection. It allows continuous remote management of devices, even when users are not logged in, and uses IPsec for secure communication with the option for multi-factor authentication. The downside is its limited compatibility, as it only works with Windows devices that support DirectAccess, and it requires IPv6 and additional network configurations.
Deploy VPN only: Implementing VPN Only offers broad compatibility with various devices and operating systems, including Windows, macOS, Linux, Android, and iOS. It is generally simpler and more straightforward to configure compared to DirectAccess and allows for flexibility in choosing VPN protocols based on security and compatibility needs. However, users must manually initiate the VPN connection each time they need access to the corporate network, and administrators typically cannot manage remote devices when they are not connected to the VPN.
In this case, I will select the option “Deploy VPN only”.
<img width="842" height="610" alt="image" src="https://github.com/user-attachments/assets/45baeb94-482c-45e0-b5f9-aff653c92a2d" />

1.	Once this option is selected, a new window called “Routing and Remote Access” will open.













In this case, you can see by the icon with the red arrow that the service is not currently configured and enabled.
To configure it, we right click on it and select the option “Configure and enable Remote Access”:

<img width="847" height="609" alt="image" src="https://github.com/user-attachments/assets/fdf86262-d1ba-4d59-9b38-654079a3988e" />












Once the wizard is opened, a configuration menu opens where I have to choose a combination of services to provide:
•	Remote Access: Configures the server to function as a Remote Access Service (RAS), allowing users to connect to the corporate network via dial-up lines or private networks.
•	Network Address Translation (NAT): Configures the server to act as a NAT device, enabling devices on a private network to access the Internet using a single public IP address. NAT hides internal IP addresses, enhancing security and conserving public IP addresses.
•	Virtual Private Network (VPN) and NAT access: Configures the server to provide both VPN and NAT services. This allows remote users to securely connect to the corporate network over the Internet (VPN) and also provides NAT services to manage network traffic between internal and external networks.
•	Secure connection between two private networks: Sets up a secure connection between two distinct networks, allowing them to communicate securely over the Internet. This is ideal for connecting different branches of a company, creating a private virtual network between them.
•	Custom configuration: Allows for detailed configuration of remote access services. You can manually select the specific services you want to configure, such as VPN, NAT, routing, dial-up remote access, etc. This option offers the most flexibility to tailor the setup to specific needs.
In this case, I select the first option (Remote Access), since I am only interested in providing the VPN service:
<img width="935" height="692" alt="image" src="https://github.com/user-attachments/assets/5de4a426-edea-4d12-9793-39ae2e4e9c6b" />

Then, I select the “VPN” option:
<img width="948" height="667" alt="image" src="https://github.com/user-attachments/assets/f954da0e-c201-464c-beb3-01cb19a2719f" />

Now, I must select the interface that connects the server to the Internet.
In this case, it is the “Ethernet” interface, whose IP address is 192.168.0.9:
 <img width="922" height="708" alt="image" src="https://github.com/user-attachments/assets/fd015702-db21-4852-a28f-f74fb8a84b45" />

Now, I must select whether I want the IP addresses of the computers connecting to the VPN to be assigned automatically (via DHCP) or whether I want an IP address to be chosen from a defined range.
In this case, I will select the option “From a specified address range”:

<img width="765" height="514" alt="image" src="https://github.com/user-attachments/assets/2d65b5c2-afd5-44ac-9679-64a3bd07dd86" />

<img width="1016" height="737" alt="image" src="https://github.com/user-attachments/assets/c3b5e3a4-f600-4999-8987-c892fe8d49fa" />









In This Case I Choose No, Use Routing and Remote Access to Authenticate Connection Request.
<img width="951" height="619" alt="image" src="https://github.com/user-attachments/assets/2e7e5c6b-221a-4869-b264-5c52b1d4b280" />

Finally, I check that everything is correct and press “Finish”:

<img width="898" height="584" alt="image" src="https://github.com/user-attachments/assets/5a8604c1-5a65-4313-8fa2-1a3ef8792713" />











Once this is done, the VPN server will be running:
<img width="1015" height="787" alt="image" src="https://github.com/user-attachments/assets/6f331b61-51ab-4f46-9338-bd53716225a5" />

Now, to connect to a VPN from a client computer (in this case, JSS-PC01), I go to the search bar and type VPN.
Then, I select the “VPN Configuration” option:
<img width="810" height="594" alt="image" src="https://github.com/user-attachments/assets/d55c26de-dce2-43e5-a97d-bce73fcb7370" />












Then, I select the “Add a VPN connection” option:














	Task Automation with PowerShell
PowerShell is a valuable tool for automating tasks in a Help Desk role because it saves time and reduces manual work. Instead of performing repetitive tasks like resetting passwords or creating user accounts one by one, you can automate these processes with scripts, completing them in seconds. This not only speeds up your workflow but also allows you to handle more requests in less time.
Automating with PowerShell also ensures consistency and reduces human error. When tasks are done manually, there’s always a risk of mistakes, but a well-written script guarantees the same accurate result every time. This makes PowerShell an essential tool for ensuring that systems are properly managed and maintained.
Additionally, PowerShell offers flexibility, allowing you to manage different aspects of IT, from Active Directory to software deployment, making it a versatile solution for a wide range of tasks.
 <img width="312" height="312" alt="image" src="https://github.com/user-attachments/assets/be2505ec-7e89-462c-a80b-11a989db8df1" />

Bulk User creation through Powershell
Using PowerShell to create users in Active Directory offers key advantages like automation, allowing repetitive tasks to be performed quickly and consistently, ensuring uniformity across the organization. It also provides scalability, making it efficient to manage large numbers of users, and flexibility to customize attributes during creation, all of which help streamline user management in enterprise environments.
To perform a mass creation of users using Powershell, I must perform the following steps:
1.	First, I am going to create a file with .csv (Comma-Separated Value) extension, which will contain the users that I am going to add to Active Directory using Powershell.
This file, as the name of the extension indicates, will separate each Active Directory attribute by commas (,).

2.	Now, I must prepare the script that imports that .csv file, and run the command that creates the users in Active Directory based on that file.


First, I import the CSV file.
<img width="1103" height="737" alt="image" src="https://github.com/user-attachments/assets/3f439c6d-d3db-4869-a976-e88276d80ef8" />

Then, I create some variables that will be useful to pass as parameters to the command that will create the users.
In this case, for testing purposes, all users will go to the same OU, and the domain will be one created on a Windows Server 2019 server, named 'Jssdomain.com'.

<img width="915" height="476" alt="image" src="https://github.com/user-attachments/assets/5b927d1a-b642-4848-a9eb-6789b46c300c" />









Once the .csv file is loaded in the $users variable, I proceed to go through each one of the users and for each one of them (for Each), I save their data in variables and pass them to the New-AD User object, which will create the users.

<img width="1012" height="999" alt="image" src="https://github.com/user-attachments/assets/f6401602-b2c1-4d4e-9846-a620e6eab484" />

<img width="1015" height="656" alt="image" src="https://github.com/user-attachments/assets/7426fcc0-2f93-4b76-bc88-0c80b023b3d8" />

 
<img width="1005" height="608" alt="image" src="https://github.com/user-attachments/assets/7688076e-7cfd-4054-b159-8e281b47c1bc" />




And the result of executing the script in Active Directory is this (the user 'Raj' was already created)

<img width="1016" height="756" alt="image" src="https://github.com/user-attachments/assets/83acd664-4747-4f4b-9043-7842007e8a03" />





	Bulk User Disabling through PowerShell
To disable users in PowerShell, the command 'Disable-AD Account' is used.
In this case, I will disable those users that belong to the 'IT' department.
The script is simple, since the command only receives the user's identity.

<img width="1016" height="1046" alt="image" src="https://github.com/user-attachments/assets/9c13831d-b72b-4f04-ade4-16b54a63004a" />











Project Conclusion
Project Overview:
This project demonstrates my capabilities in setting up and managing a comprehensive IT environment, essential for a role as an IT Support Specialist or Help Desk Jr/Trainee. The core of the project is the creation of a homelab that simulates real-world IT infrastructure, showcasing a range of skills from system administration to network monitoring, user support, and cloud-based services.
Tools and Technologies Used:
1.	Virtualization:
o	Hyper-V: Used to create and manage multiple virtual machines (VMs), allowing for a dynamic and scalable test environment.
<img width="313" height="313" alt="image" src="https://github.com/user-attachments/assets/634f9597-f41a-4ba5-a029-2f8c84807e4c" />

 
1.	
Operating Systems:
o	Windows Server 2022: Deployed as domain controllers and application servers.

<img width="313" height="313" alt="image" src="https://github.com/user-attachments/assets/e94338ac-41f9-4718-baef-0315611424f2" />






•	Windows 10: Set up as client              machines.

<img width="312" height="313" alt="image" src="https://github.com/user-attachments/assets/12eb7b85-c586-4d9b-998b-4a4ac0e11368" />


Directory Services:
•	Active Directory (AD): Implemented to manage users, groups, and devices across the network.

<img width="313" height="312" alt="image" src="https://github.com/user-attachments/assets/76f07054-94a5-4006-8d17-4f3563d2b934" />






Azure Active Directory: Set up for cloud-based identity and access management.


<img width="313" height="313" alt="image" src="https://github.com/user-attachments/assets/28c91505-a70f-44f4-8348-eb0153a4f143" />





Network and System Management:
•	PDQ Deploy: Utilized for automated software deployment and management.

<img width="313" height="312" alt="image" src="https://github.com/user-attachments/assets/0c3379c5-afff-451b-a372-993a1c29f66f" />





Cloud Services:
•	Microsoft 365: Implemented and managed user accounts, licenses, and shared mailboxes. Configured services like Exchange, SharePoint, and OneDrive, including recovery of deleted files and mailbox management.
 <img width="313" height="313" alt="image" src="https://github.com/user-attachments/assets/a776dd67-2e25-4b36-9616-76b52a4baad4" />

Ticketing System:
•	Jira Service Management (JSM): Adopted to simulate real-world IT ticketing and service management.


<img width="312" height="313" alt="image" src="https://github.com/user-attachments/assets/a991eb8e-8fc4-4eff-a8b1-2a6f7b426845" />





Remote Access and Support:
•	TeamViewer: Integrated for remote access and support capabilities, facilitating troubleshooting and user assistance.

<img width="312" height="313" alt="image" src="https://github.com/user-attachments/assets/8bb36a5e-af72-4c54-9c73-0122d02bcb30" />








VPN Configuration:
•	Remote Access Role (Windows Server): Set up a VPN to allow secure remote access to the network, including the configuration of roles and client connectivity.
 <img width="277" height="277" alt="image" src="https://github.com/user-attachments/assets/c85f3659-9a10-48b2-8cdd-12a084ba779f" />

Task Automation:
•	PowerShell: PowerShell scripts were used to automate key tasks such as bulk user creation and bulk user disabling in Active Directory, improving efficiency and consistency across the project.
 <img width="280" height="280" alt="image" src="https://github.com/user-attachments/assets/91776bb9-f8cf-4e3a-a33f-d3e6411cf2ac" />

Key Project Components:
1.	Homelab Setup:
o	Created a fully functional Homelab consisting of two Windows Server 2022 VMs, four Windows 10 client VMs, and a Linux VM.
o	Configured the VMs to replicate a corporate IT environment, including domain joining and policy application.
2.	Active Directory Deployment and Management:
o	Set up a primary domain controller with Windows Server 2022.
o	Implemented organizational units (OUs) to mirror departmental structures.
o	Created user accounts and assigned them to appropriate OUs, simulating real-world departmental hierarchies.
o	Integrated Group Policy Objects (GPOs) to enforce security policies, manage desktop configurations, and ensure uniformity across the domain.
3.	Cloud Identity and Security:
o	Established and configured Azure Active Directory (Azure AD) for cloud-based identity management.
o	Created and managed cloud-based user accounts and groups.
o	Enabled Multi-Factor Authentication (MFA) for added security on Azure AD.
4.	System and Software Management:
o	Utilized PDQ Deploy for centralized software deployment across all Windows clients, demonstrating proficiency in remote software management.
o	Configured and managed Windows Server roles and features, including DNS, DHCP, and Remote Access.
5.	Microsoft 365 Administration:
o	Managed Microsoft 365 services, including user accounts, license assignments, and group configurations.
o	Administered Exchange Online for mailbox management, and implemented SharePoint and OneDrive for file storage and collaboration.
o	Executed recovery operations for deleted files and mailboxes.
6.	Network Monitoring and Visualization:
o	Installed and configured Zabbix for comprehensive monitoring of all VMs.
o	Set up Grafana dashboards to visualize monitoring data, providing insights into system performance and health.
7.	IT Service Management:
o	Implemented Jira Service Management (JSM) as a ticketing system to simulate handling IT support requests.
o	Managed and resolved simulated tickets, reflecting typical IT support tasks like password resets, software installations, and system troubleshooting.
8.	Remote Access and Support:
o	Integrated tools like TeamViewer and AnyDesk for remote access and support capabilities.
o	Demonstrated proficiency in using these tools to resolve user issues remotely, enhancing support efficiency.

9.	VPN Configuration:
o	Configured a VPN to provide secure remote access to network resources, including role-based access controls and client setup.
10.	Task Automation with PowerShell:
o	Automated key tasks such as bulk user creation and bulk user disabling in Active Directory, reducing manual intervention and improving efficiency.



