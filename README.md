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

