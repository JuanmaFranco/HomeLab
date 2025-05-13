# IT Support and Administration Home Lab - Personal Project

![Home Lab Logo](images/homelab-logo.png)

Welcome to my comprehensive IT infrastructure project, where I have meticulously designed and implemented a homelab environment integrating Active Directory solutions, both on-premises and in the cloud through Azure Active Directory. This project highlights my proficiency in deploying and managing complex IT systems, utilizing a range of tools and skills to ensure robust operational capabilities.

This project not only showcases my technical expertise in IT infrastructure management but also underscores my ability to design, deploy, and optimize enterprise-grade solutions. It reflects my commitment to excellence in IT service delivery and my readiness to contribute effectively to organizational IT operations.      

-----------------------------------------------------------------------------------------------------

# Table of Contents

1. [Homelab Creation](#homelab-creation)
2. [User and Group Creation](#user-and-group-creation)
3. [Microsoft 365 Integration and Management](#microsoft-365-integration-and-management)
4. [Group Policy Management and Implementation](#group-policy-management-and-implementation)
5. [Password Reset](#password-reset)
6. [Multi-factor Authentication](#multi-factor-authentication-mfa)
7. [Software Deployment](#software-deployment)
8. [Remote Access and Troubleshooting](#remote-access-and-troubleshooting)
9. [VPN Configuration](#vpn-configuration)
10. [Monitoring and Visualization](#monitoring-and-visualization)
11. [Task Automation with PowerShell](#task-automation-with-powershell)
12. [Project Conclusion](#project-conclusion)

-----------------------------------------------------------------------------------------------------

## Homelab Creation

In this homelab setup, we will be using Hyper-V to create a virtualized environment. The primary components of this setup will include:

![Equipment](images/equipment.png)

1. **Three Servers**:
    - One server (Windows Server 2022) will act as the **Domain Controller (DC)**, responsible for managing the Active Directory (AD) and handling authentication and authorization within the domain.
    - The second server (Windows Server 2022) will be used for other purposes such as file storage, applications, or additional services required within the network.
    - Additionally, a Linux server (Debian 12.5) will be implemented for monitoring purposes. This server will monitor the network and all devices within it.

2. **Four Client Machines**:
    - These machines will be used to simulate end-user workstations within the network. Each machine will be joined to the domain controlled by the Domain Controller.
  
    - Naming Convention
        - Prefix: JMFSOFT
        - Client Identifier: PC
        - Unique Number: 01, 02, 03, 04

        Detailed Naming List:
        1. JMFSOFT-PC01
        2. JMFSOFT-PC02
        3. JMFSOFT-PC03
        4. JMFSOFT-PC04

### Organizational Units (OUs) for Departments

To keep our domain structure well-organized, we will create Organizational Units (OUs) for each department. This will help us manage users, computers, and other resources efficiently within Active Directory. The following OUs will be created:
     
![Organizational Units](images/organizational-units.png)

1. **IT** - Contains users such as IT Support, Sysadmins, and Network Engineers.
2. **Finance** - Contains users such as Finance Managers and Finance Analysts.
3. **Sales** - Contains Sales Representatives.
4. **HR** - Contains the HR Manager and related personnel.
5. **Marketing** - Contains Marketing Specialists and Content Writers.
6. **Development** - Contains Developers and DevOps Engineers.
7. **Customer Service** - Contains Customer Service Representatives.
8. **Design** - Contains Graphic Designers.
9. **Administration** - Contains the Office Manager and administrative staff.

### Summary of Steps

| Step | Description |
|------|-------------|
| **1. Set up Hyper-V on your host machine** |   |
|     1.1 | **Install Hyper-V**: Open the Control Panel, navigate to Programs > Programs and Features > Turn Windows features on or off, check the box for Hyper-V, click OK, and restart your computer. <br/><br/> ![Hyper-V](images/hyper-v.PNG) |
|     1.2 | **Configure Hyper-V**: Open Hyper-V Manager from the Start menu, select your host machine, and configure virtual switches by going to Virtual Switch Manager in the right-hand Actions menu. Create a new External virtual switch to allow VMs to access your physical network. <br/><br/> ![External Virtual Switch](images/external-virtual-switch.PNG)|
| **2. Create Virtual Machines** |   |
|     2.1 | **Create the Domain Controller VM**: In Hyper-V Manager, click New > Virtual Machine, and follow the wizard to set up the VM with appropriate settings such as name, generation, memory, network, and virtual hard disk. Install an operating system later. <br/> In my case, I'm going to choose to change the default path to store the virtual machine, as it seems more organized to me. In this case, I'll opt to place it in C:\Hyper-V\VMs\\. <br/><br/> ![DC01-1](images/dc01-1.PNG) <br/><br/> Afterwards, I select Generation 1, as Generation 2 can be prone to errors. <br/><br/> ![DC01-2](images/dc01-2.PNG) <br/><br/> Now, I need to allocate the RAM to the virtual machine. In this case, I'll opt for the minimum recommended for Windows Server 2022 with Desktop Experience, which is 4GB (4096 MB). <br/> Additionally, I'll choose to leave the **'Use dynamic memory for this virtual machine'** checkbox checked. <br> When a virtual machine uses dynamic memory, it means that its memory allocation can adjust automatically based on the demand from the operating system and applications running within the virtual machine. Instead of assigning a fixed amount of memory, the virtual machine can request and release memory as needed, allowing for more efficient utilization of host resources. <br/><br/> ![DC01-3](images/dc01-3.PNG) <br/><br/> Then, I select the network adapter to which the virtual machine will connect. In this case, it's the network adapter created in step 1.2 (WAN), which is associated with my Qualcomm Atheros QCA9377 wireless network card. <br/><br/> ![DC01-4](images/dc01-4.PNG) <br/><br/> Next, I will create a virtual hard disk of 2TB (2048GB), which will be more than enough for any practice you may want to perform. <br/><br/> ![DC01-5](images/dc01-5.PNG) <br/><br/> Finally, I select the option 'Install an operating system from a bootable CD/DVD-ROM' and locate the location of the .ISO file containing the Windows Server 2022 Operating System. <br/><br/> ![DC01-6](images/dc01-6.PNG) <br/><br/> Once this is done, the virtual machine is successfully created. Now, all that's left is to connect to it, start it up, and follow the Windows Server installer. <br/><br/> ![DC01-7](images/dc01-7.PNG) |
|     2.2 | **Create the Second Server VM**: Follow the same steps as above but name this VM Server2. <br/><br/> Next, I create the second virtual machine, which will also have Windows Server 2022 as its operating system. I will avoid detailing the installation as it is the same as the DC01 virtual machine, only the name is different. <br/> In this case, the virtual machine will be named 'SV02'. <br/><br/> ![SV02](images/sv02.PNG) <br/><br/> Then, once all the fields are filled out, I have the 2 server machines created. <br/><br/> ![SV03](images/sv02-2.PNG) |
|     2.3 | **Create the Client VMs**: Repeat the VM creation process four times for client machines, naming them JMFSOFT-PC01, JMFSOFT-PC02, JMFSOFT-PC03, and JMFSOFT-PC04. Assign at least 1024 MB of memory to each client VM. <br/><br/> Now, I repeat the process but instead of using the Windows Server 2022 .iso file as the boot disk, I choose the Windows 10 .iso file. <br/><br/> ![Client1](images/client1.PNG) <br/><br/> ![Client1-2](images/client1-2.PNG) <br/><br/> Then, once the 2 servers (Windows Server 2022) and the four client machines (Windows 10) are created, I have the following: <br/><br/> ![Client1-3](images/client1-3.PNG)|
| **3. Install the necessary operating systems on all VMs** |   |
|     3.1 | **Prepare installation media**: Obtain ISO files for Windows Server (for the servers) and Windows 10/11 (for the client machines). <br/><br/> In this case, I chose to download both .iso files (Windows Server 2022 and Windows 10) beforehand, but you can also choose the option 'Install an operating system later,' which creates the virtual machine and waits for you to attach the desired .iso file later. <br/><br/> If you prefer to do it that way, you can download the .iso files from the official Microsoft website: <br/><br/> -  [Windows Server 2022](https://www.microsoft.com/es-ES/evalcenter/evaluate-windows-server-2022)<br/> -  [Windows 10](https://www.microsoft.com/es-es/software-download/windows10) <br/> |
|     3.2 | **Install Windows Server on DC1 and Server2**: Start the VM, connect to it, attach the Windows Server ISO, and follow the installation prompts to install Windows Server. Configure the server with appropriate settings (e.g., server name, IP address). <br/><br/> Once the virtual machine is created and the Windows Server 2022 disk is 'inserted,' I will start it and proceed with the installation. I will perform this process for both the DC01 (Domain Controller) server and the SV02 server. <br/><br/> ![DC-INS-1](images/dc-ins-1.PNG) <br/><br/> After clicking 'Install,' I need to select the version I want. Among the options are the Standard and Datacenter versions, each with a Desktop Experience (GUI) version and a Server Core version. In this case, I will select the Standard version with Desktop Experience: <br/><br/> ![DC-INS-2](images/dc-ins-2.PNG) <br/><br/> Next, I need to select the partition on which the operating system will be installed. In this case, it is the only partition I have: the 2TB partition created when I created the virtual machine, but I could also create another partition and install it there. <br/><br/> ![DC-INS-3](images/dc-ins-3.PNG) <br/><br/> After selecting the partition, the installer will continue copying files and installing the operating system. The installation time varies depending on the resources, both those we have and those allocated to the virtual machine. <br/><br/> ![DC-INS-4](images/dc-ins-4.PNG) <br/><br/> Now, once the above is completed, you will be prompted to create a password for the 'Administrator' user, who has full control over the operating system. This account is similar to the superuser in Linux. It is essential to remember this password. <br/><br/> ![DC-INS-5](images/dc-ins-5.PNG) <br/><br/> Now, I can log in with the credentials provided recently. <br/><br/> ![DC-INS-6](images/dc-ins-6.PNG) <br/><br/> Once logged in, the 'Server Manager' will automatically open (this can be disabled), which allows managing Roles and Features, configuring the server's Firewall, changing the name, changing the IP address, managing storage, etc. <br/><br/> In this case, within the 'Local Server' section, I will change its name and IP address. The IP address will change from being managed by DHCP to static, which is seen as a good practice (in servers, not in client machines), as it increases reliability among other reasons. <br/><br/> ![DC-INS-7](images/dc-ins-7.PNG) <br/> <br/> The **name** will be: DC01. <br/><br/> ![DC-INS-8](images/dc-ins-8.PNG) <br/><br/> The **static IP address** for DC01 will be: 192.168.1.5 <br/> The **subnet mask** will be: 255.255.255.0, which means that the first 3 octets of the IP address (192.168.1) will correspond to identifying the network portion, while the fourth and last octet will correspond to identifying the host portion. In this case, with the /24 subnet mask, we get 2^8 - 2 (254) available hosts for the devices and/or servers (2 raised to the available bits, minus 2 because there are 2 addresses that cannot be assigned: 192.168.1.0, which corresponds to the network address, and 192.168.1.255, which corresponds to the broadcast address). <br/> The **default gateway** will be 192.168.1.1, corresponding to the IP address of the default gateway that I have. <br/> The **primary DNS server** will be: 8.8.8.8, corresponding to Google's public DNS server. <br/> **The alternate DNS server** will be: 8.8.4.4, also corresponding to another public DNS server provided by Google. <br/><br/> ![DC-INS-9](images/dc-ins-9.PNG) <br/><br/> To apply the changes (the server name change), it is necessary to restart the computer. <br/><br/> That's all for the DC01 server (temporarily, although it's still not a domain controller). Now, I will proceed to install the Windows Server operating system on the other server machine (SV02).  |
|     3.3 | **Install Windows 10/11 on client VMs**: Start each client VM, connect to it, attach the Windows 10/11 ISO, and follow the installation prompts to install Windows. Configure each client with appropriate settings (e.g., machine name, IP address). <br/><br/> Once the Windows 10 disk is 'inserted' into the client machine, I proceed to start it to initiate the installation process. <br/><br/> ![Win-01](images/win-01.PNG) <br/><br/> Then, I press "Install now," and enter the product key (if available). In this case, I don't have one, so I select "I don't have a product key. <br/><br/> ![Win-02](images/win-02.PNG) <br/><br/> Now, I select the version of Windows that I want to install. It's crucial to choose the Pro or Education version, as the Home version lacks the capability to join an Active Directory domain, thus cannot be centrally managed. In this case, I will opt for the Windows 10 Pro version. <br/><br/> ![Win-03](images/win-03.PNG) <br/><br/> Now, I select again the partition on which I want to install the operating system. In this case, it's the only partition I have created. <br/><br/> ![Win-04](images/win-04.PNG) <br/><br/> Now, I must wait for the installation process to finish. <br/><br/> ![Win-05](images/win-05.PNG) <br/><br/> That's it. Now, once the operating system has started, I complete the final configurations (region, keyboard, network, local account, privacy, etc.), and once the desktop is displayed, I proceed to change the computer name to "JMFSOFT-PC01": <br/><br/> ![Win-06](images/win-06.PNG) <br/><br/> Finally, I repeat the process for the rest of the computers, only varying the name (JMFSOFT-PC02, JMFSOFT-PC03, and JMFSOFT-PC04). |
| **4. Configure the Domain Controller** |   |
|     4.1 | **Install Active Directory Domain Services (AD DS):** Log in to DC1, open Server Manager, click Add roles and features, select Active Directory Domain Services, and complete the installation. <br/><br/> Now, to be able to use the Windows Directory Service (**Active Directory**), I need to install Active Directory Domain Services (AD DS) on the server designated as the domain controller. To do this, I go to the Server Manager and select the 'Add Roles and Features' option. Then, I select Active Directory Domain Services (AD DS) and follow the steps. <br/><br/> ![AD-01](images/ad01.png) <br/><br/> In the next window, I select the 'Role-based or feature-based installation' option and also verify in the top left corner that the destination server matches the server where I want to install the role or feature (in this case, DC01, which is correct). <br/><br/> ![AD-02](images/ad02.png) <br/><br/> Again, I select the desired server (DC01) and click Next. <br/><br/> ![AD-03](images/ad03.png) <br/><br/> Next, I select the Active Directory Domain Services option. Clicking on this option will open an additional window that will identify all the roles and features that will be installed and are necessary for the proper functioning of this role. <br/><br/> ![AD-04](images/ad04.png) <br/><br/> ![AD-05](images/ad05.png) <br/><br/> After clicking the 'Add Features' option, the original Active Directory Domain Services checkbox will appear checked. <br/><br/> ![AD-06](images/ad06.png) <br/><br/> Next, the features that can be installed on the server will appear. In this case, I don't need to add any, so I click Next. <br/><br/> ![AD-07](images/ad07.png) <br/><br/> Finally, a summary of the installed roles and features will appear. I can also check the box indicating to the server that it should restart if necessary to complete the installation. This box should be checked carefully, as it is not appropriate to restart a server that is in operation in a business environment unless this restart has been scheduled in advance and does not affect the proper operation of any service. Once verified that everything is correct, I press 'Install' to begin the installation of AD DS. <br/><br/> ![AD-08](images/ad08.png) <br/><br/> Once this is done, the installation will begin. I can close the window and let the installation run in the background or simply wait for it to finish. <br/><br/> ![AD-09](images/ad09.png) <br/><br/> Finally, the installation is complete, but this doesn't mean that the server is already a Domain Controller, as only the Active Directory Domain Services (AD DS) were installed. To accomplish this, I must promote the server to a Domain Controller (DC), which will be done in the next step. <br/><br/> ![AD-10](images/ad10.png) <br/><br/> |
|     4.2 | **Promote DC1 to a Domain Controller:** After installation, click on the notification flag in Server Manager, select Promote this server to a domain controller, choose Add a new forest, enter a root domain name (e.g., jmsoft.local), and complete the wizard. Restart the server as prompted. <br/><br/> Now, once Active Directory Domain Services are installed, I need to promote the server to a domain controller to create the forest, the domain, and then add the desired users. To do this, I need to go to the notification that appears in the Server Manager and click on 'Promote this server to a domain controller.' <br/><br/> ![AD-11](images/ad11.png) <br/><br/> Once that option is selected, a window will open with three options: <br/><br/> **1 - Add a domain controller to an existing domain**: This option is useful when you want to add an additional server that provides authentication and authorization services within the same domain (a previously created domain). This increases redundancy and availability. It also helps distribute the load among multiple Domain Controllers, improving performance. <br/><br/> **2 - Add a new domain to an existing forest**: This involves creating an additional domain within an Active Directory forest. A forest is a collection of one or more domains that share a common schema and global catalog configuration. Domains within the same forest have implicit transitive trust relationships, allowing for authentication and access to resources across domains. <br/><br/>**3 -  Add a new forest**: This means creating a completely independent instance of Active Directory. A forest is the highest security and administrative boundary in AD. <br/><br/> In this case, I will select the last option (Add a new forest), and the root domain name will be '**jmfsoft.local**'. If there is a website called 'jmfsoft.com', it is good practice not to use that domain name as the root domain name to avoid DNS resolution conflicts. <br/><br/> ![AD-12](images/ad12.png) <br/><br/> Once 'Next' is pressed, a window will open with multiple options: <br/><br/> ![AD-13](images/ad13.png) <br/><br/> Forest functional level and domain functional level refer to the compatibility settings and features available in an Active Directory environment. These levels determine which Active Directory functions and features can be used, based on the versions of Windows Server running on the domain controllers in the environment. <br/><br/>1 - The "**Domain functional level**" specifies the features and functionalities available within a specific domain in Active Directory. <br/><br/>2 - The "**Forest functional level**" specifies the features and functionalities available across the entire Active Directory forest, which includes all domains within that forest.<br/><br/> By raising the functional levels of the domain or forest, all domain controllers must be running at least the minimum version of Windows Server required by the new functional level. Once the functional level of the forest or domain is raised, it cannot be reverted to a previous functional level without restoring from a backup. <br/><br/> Next, there is a section called 'Specify Domain Controller Capabilities,' which has three options: <br/><br/> **1 - Domain Name System (DNS) Server**: This option allows the Domain Controller to provide name resolution services that are essential for Active Directory functionality. DNS is critical for clients and servers to locate resources within the domain. AD depends on DNS to locate domain controllers, replication services, and other critical resources. SRV (Service Location) records in DNS enable clients to locate services such as domain controllers. <br/><br/>**2 - Global Catalog (GC)**: This option indicates that the domain controller contains a partial copy of all objects in the Active Directory forest. Domain controllers acting as GCs store key information and allow quick searches across the entire forest. During logon, the GC can provide information about universal group membership, which is crucial for user authentication and authorization. <br/><br/>**3 - Read-Only Domain Controller (RODC)**: This is a special type of domain controller designed for environments where the physical security of the server cannot be guaranteed, such as branch offices or remote locations. The RODC stores a read-only copy of the Active Directory database. It cannot make changes to AD, which helps protect the integrity of the directory if the server is physically compromised. By default, the RODC does not store user credentials (passwords) unless explicitly configured to do so. <br/><br/> Finally, we need to specify the **Directory Services Restore Mode (DSRM) password**, which is an important security measure in Active Directory domain controllers. It is used in directory recovery situations when the system is in a degraded state or when critical maintenance tasks need to be performed. The DSRM password allows logging in to a domain controller even if the operating system cannot start normally or if the Active Directory service is inaccessible. This provides emergency access to the domain controller in critical situations. <br/><br/> ![AD-14](images/ad14.png) <br/><br/> Next, we must verify the NETBIOS name, which is a network identifier used in Microsoft operating systems to identify resources on a local network. In the context of Active Directory, the NetBIOS name is primarily used for backward compatibility and to enable communication with legacy systems that use this technology. <br/><br/> ![AD-15](images/ad15.png) <br/><br/> Next, the paths will be displayed, indicating the location of the folder containing the database, the folder containing the log files, and the SYSVOL folder. <br/><br/>1 - **The database folder (C:\Windows\NTDS by default)** stores the Active Directory database, where all directory objects and attributes are stored, including users, groups, policies, and system configurations. The NTDS.DIT file is the primary Active Directory database file. It contains all directory data, such as users, groups, computer objects, etc. Active Directory domain controllers (DCs) access this database to provide directory services.<br/><br/>2 - **The log files folder (C:\Windows\NTDS by default)** contains the transaction log files that record all operations performed on the Active Directory database. Transaction log (LOG) files are sequential and are used to ensure the consistency and integrity of the Active Directory database. Each transaction performed in the database is recorded in these files before being committed and written to the database. In the event of system failure or data loss, the log files can be used to recover lost data or to restore the database to a consistent state.<br/><br/> 3 - **The SYSVOL folder (C:\Windows\SYSVOL by default)** stores shared data and group policies in an Active Directory environment. SYSVOL contains files and folders that are replicated among all domain controllers in the domain. This includes user login scripts, login policies, group policies, login script files, etc.<br/><br/> In summary, these folders are critical for the operation and integrity of an Active Directory environment. The database folder stores the AD database, the log files folder records transactions, and the SYSVOL folder stores shared data and group policies. <br/><br/> ![AD-16](images/ad16.png) <br/><br/> Then, a summary of all the selected options and configurations is displayed. This allows reviewing the choices to confirm if I want to make a last-minute change before promoting the server as a domain controller. In my case, I reviewed all the options and they are correct, so I will proceed with the installation. <br/><br/> Additionally, I can copy the executed script which contains the commands executed by the operating system to perform all the above. This is useful if I want to run a script in Windows PowerShell and automate future installations. <br/><br/> ![AD-17](images/ad17.png) <br/><br/> As a final step prior to installation (i.e., promoting the server to Domain Controller), prerequisites are checked to see if the system is suitable for promotion. In this case, although it threw some usual warnings, the system is suitable to be promoted to a domain controller. After this, I click on Install and wait for the installation to complete. When the promotion operation is finished, the system will automatically restart. <br/><br/> ![AD-18](images/ad18.png) <br/><br/> After the server restarts, the login screen changes. Below the user and password, a message appears saying **'Sign in to JMFSOFT'**, indicating that, unless specified otherwise, I'll sign in to the domain with that name. This doesn't necessarily mean that the server is a domain controller, as the login process on client machines will be the same, but it does indicate that the server is now part of the JMFSOFT domain. <br/><br/> ![AD-19](images/ad19.png) <br/><br/> After promoting the server to a domain controller, new sections will appear in the Server Manager that didn't exist before. For example, on the left side, you'll see AD DS (Active Directory Domain Services) and DNS (Domain Name System). Then, in the tools section, several new tools will appear, such as Active Directory Administrative Center (ADAC), DNS, Active Directory Domains and Trusts, Active Directory Sites and Services, Active Directory Users and Computers (ADUC), among others. <br/><br/> With this, I conclude step 4.2, which aims to promote the server to a domain controller. |
| **5. Join all client machines to the domain** |   |
|     5.1 | **Configure network settings:** Ensure that all client machines and the second server can resolve the domain by setting the DNS server to the IP address of DC1. <br/><br/> To join client computers (and also the secondary server) to the newly created domain (jmfsoft.local), the first thing I need to do is change the DNS server to the IP address of the domain controller. This is necessary for several reasons: <br/><br/>1 - **Name Resolution**: Active Directory relies on the DNS service to resolve domain names, such as the domain name you are joining. The DNS server configured on the client needs to correctly resolve these names to locate the domain and complete the join process. <br/><br/>2 - **Domain Controller Location**: The client needs to locate the Domain Controller to join the domain and obtain configuration information. The IP address of the Domain Controller is used to identify where the Active Directory infrastructure is located on the network. <br/><br/>3 - **Resource Records in DNS**: During the domain join process, the client needs to look up specific resource records (e.g., SRV records) in DNS to locate Active Directory services, such as authentication and replication services. These records are associated with the domain name and are resolved via the DNS server configured on the client.<br/><br/>4 - **Communication with the Domain Controller**: After joining the domain, the client must be able to communicate with the Domain Controller to authenticate users, obtain group policies, and perform other Active Directory operations. Configuring the DNS server is essential for the client to correctly communicate with the Domain Controller. <br/><br/> If I do not point the DNS server to the IP address of the server, I get the following error: <br/><br/> ![Dom-1](images/dom-1.png) <br/><br/> So, to solve this issue and allow the client machines (and the additional server) to join the 'jmfsoft.local' domain, I go to the network settings and set the DNS server to the IP of the domain controller, which in this case is 192.168.1.5. Although in some parts of the project the network is shown as both 192.168.0.0/24 and 192.168.1.0/24, this is because I am doing it in two geographically distant locations with two different ISPs, so this number will vary at times, but the concept remains the same. <br/><br/> Therefore, to do this, I go to the wireless connection icon -> Network and Sharing Center -> Change adapter settings -> Select the network adapter and right-click on it -> Properties -> Internet Protocol Version 4 -> Properties and then set the DNS server address to match the domain controller's IP address. <br/><br/> ![Dom-2](images/dom-2.png) <br/><br/> |
|     5.2 | **Join each client to the domain:** On each client machine, open System Properties (right-click on This PC > Properties > Advanced system settings), click on Change next to To rename this computer or change its domain or workgroup, select Domain, enter the domain name (e.g., jmsoft.local), provide the credentials of a domain user, and restart each client machine after joining the domain. <br/><br/> Once this is done, I should be able to join the domain correctly, as the client computer can now correctly resolve the domain name. Now, I will try to join again through 'Change the name of this PC (Advanced):' <br/><br/> ![Dom-3](images/dom-3.png) <br/><br/> Once the network configurations are completed, I need to enter credentials. The user account I need to enter must have at least 'Add computer to the domain' permissions. This permission can be granted by a Domain Admin. <br/><br/> ![Dom-4](images/dom-4.png) <br/><br/> Once the credentials are entered, a message will appear indicating that the computer has been successfully joined to the domain.  <br/><br/> ![Dom-5](images/dom-5.png) <br/><br/> For this to take effect, simply restart the computer and log in with an account that belongs to the 'jmfsoft.local' domain: <br/><br/> ![Dom-6](images/dom-6.png) <br/><br/> Now, at the next login, the same message that appeared on the Domain Controller will appear: 'Sign in to JMFSOFT: <br/><br/> ![Dom-7](images/dom-7.png) <br/><br/> Simply enter the credentials of a user who belongs to the domain, and step 5.2 is complete. Next, I will perform the same process (joining devices to the domain) with the rest of the client computers and the additional server. |
| **6. Create Organizational Units (OUs) in Active Directory for each department** |   |
|     6.1 | **Open Active Directory Users and Computers (ADUC) on DC1**: Go to Server Manager > Tools > Active Directory Users and Computers (ADUC) or Active Directory Administrative Center (ADAC). <br/><br/> **1 - ADAC (Active Directory Administrative Center):** <br/><br/> ![ADAC1](images/adac1.png) <br/><br/> **Modern User Interface**: ADAC has a more modern and intuitive user interface, based on PowerShell and Windows Presentation Foundation (WPF) technologies. It was introduced with Windows Server 2008 R2. <br/><br/> **Advanced Features**: It offers advanced features such as the Active Directory Recycle Bin, which allows the recovery of deleted objects without having to restore the entire system. It enables more easily configurable Role-Based Access Control (RBAC). It incorporates PowerShell Search and command history, which facilitates task automation. <br/><br/> **Improved Security**: It supports smart card authentication and other enhanced authentication methods. <br/><br/> **Simplified Management**: It provides a consolidated and simplified view to manage Active Directory objects, such as users, groups, organizational units, and others. <br/><br/> **2 - ADUC (Active Directory Users and Computers):** <br/><br/> ![ADUC-1](images/aduc1.png) <br/><br/> **Classic User Interface**: ADUC has a more traditional user interface, based on the Microsoft Management Console (MMC) snap-in. It is an older tool compared to ADAC and has been used since Windows 2000. <br/><br/> **Basic Features**: It allows basic management of users, groups, computers, and organizational units in Active Directory. It does not have direct access to some of the more advanced features available in ADAC, such as the AD Recycle Bin. <br/><br/> **Common Use in Classic Scenarios**: Although it is more basic, it is still widely used for daily administration tasks due to its familiarity and simplicity. <br/><br/> **Compatibility**: It is compatible with earlier versions of Windows Server and remains a reliable tool for direct management of objects in Active Directory. <br/><br/> |
|     6.2 | **Create OUs**: Right-click on the domain (e.g., jmsoft.local) and select New > Organizational Unit. Create OUs for each department (e.g., IT, Finance, Sales, HR, Marketing, Development, Customer Service, Design, Administration). <br/><br/> To create organizational units (OUs), I have two options: do it from Active Directory Users and Computers (ADUC) or from Active Directory Administrative Center (ADAC): <br/><br/> **1 - Create from Active Directory Users and Computers (ADUC)**: To do this, I go to Server Manager, Tools, and select Active Directory Users and Computers. Then, once inside ADUC, I expand the domain (jmfsoft.local) and select where I want to create the organizational unit. <br/><br/> ![OU-1](images/ou1.png) <br/><br/> It is important to note that organizational units (OUs) cannot be created within containers, nor can group policies (GP) be applied to them. In this case, I will create an organizational unit called 'Argentina' within the domain and then create within it all the OUs related to the departments of the company at the Argentina site (assuming it is a company with many locations): <br/><br/> ![OU-2](images/ou2.png) <br/><br/> **2 - Create from Active Directory Administrative Center (ADAC)**: To do this, I go to Server Manager, Tools, and select Active Directory Administrative Center. Then, once inside, I select the location, right-click, New, and select 'Organizational Unit'. <br/><br/> ![ADAC-2](images/adac2.png) <br/><br/> Then, I complete the creation of the company's organizational units (IT, HR, Finance, etc.). <br/><br/> ![ADAC-3](images/adac3.PNG) <br/><br/> |
|     6.3 | **Move user accounts to OUs**: After creating user accounts, move each account to the appropriate OU by right-clicking on the user, selecting Move, and choosing the appropriate OU. <br/><br/> Although user creation is part of the next section, I will create a test user and move it to a previously created organizational unit (in this case, IT) just to complete point 6.3 and finish with section 1 (Homelab Creation). <br/><br/> In this case, I will create the user named 'Prueba' in Customer Service and then move it to the IT Organizational Unit. <br/><br/> ![NewUser](images/newuser.PNG) <br/><br/> Then, to move the user, I simply right-click on it and select 'Move...'. Now, I just need to choose which organizational unit (OU) to move it to. In this case, I will move it to IT: <br/><br/> ![NewUser2](images/newuser2.PNG) <br/><br/> Once done, the user has been successfully moved from 'Customer Service' to 'IT'. <br/><br/> ![NewUser3](images/newuser3.PNG) |

-------------------------------------------------------------------------------------------------

## User and Group Creation

### User Accounts and Domain Access

Although we are creating only four virtual machines to simulate end-user workstations, we will create 20 distinct user accounts within Active Directory. This approach avoids the unnecessary complexity and resource consumption of creating 20 separate machines, while still allowing us to manage and test all 20 user accounts effectively. Each user will be able to log into any of the four client machines and access the domain as if they were on a unique machine.

### User List

1. **Juan Martín Franco**
   - **Username:** juanma
   - **Occupation:** IT Support
   - **Email:** juanmafranco@jmfsoft.com
   - **Phone:** 2325 65 1813
   - **Department:** IT
   - **Location:** Argentina

2. **Bob Smith**
   - **Username:** bsmith
   - **Occupation:** Sysadmin
   - **Email:** bob.smith@jmfsoft.com
   - **Phone:** (555) 123-4562
   - **Department:** IT
   - **Location:** San Francisco

3. **Carol Davis**
   - **Username:** cdavis
   - **Occupation:** Finance Manager
   - **Email:** carol.davis@jmfsoft.com
   - **Phone:** (555) 123-4563
   - **Department:** Finance
   - **Location:** Chicago

4. **David Brown**
   - **Username:** dbrown
   - **Occupation:** Sales Representative
   - **Email:** david.brown@jmfsoft.com
   - **Phone:** (555) 123-4564
   - **Department:** Sales
   - **Location:** Miami

5. **Eve Miller**
   - **Username:** emiller
   - **Occupation:** HR Manager
   - **Email:** eve.miller@jmfsoft.com
   - **Phone:** (555) 123-4565
   - **Department:** HR
   - **Location:** Los Angeles

6. **Frank Wilson**
   - **Username:** fwilson
   - **Occupation:** IT Support
   - **Email:** frank.wilson@jmfsoft.com
   - **Phone:** (555) 123-4566
   - **Department:** IT
   - **Location:** New York

7. **Grace Moore**
   - **Username:** gmoore
   - **Occupation:** Marketing Specialist
   - **Email:** grace.moore@jmfsoft.com
   - **Phone:** (555) 123-4567
   - **Department:** Marketing
   - **Location:** Boston

8. **Hank Taylor**
   - **Username:** htaylor
   - **Occupation:** Developer
   - **Email:** hank.taylor@jmfsoft.com
   - **Phone:** (555) 123-4568
   - **Department:** Development
   - **Location:** San Francisco

9. **Ivy Anderson**
   - **Username:** ianderson
   - **Occupation:** Product Manager
   - **Email:** ivy.anderson@jmfsoft.com
   - **Phone:** (555) 123-4569
   - **Department:** Product
   - **Location:** Seattle

10. **Jack Thomas**
    - **Username:** jthomas
    - **Occupation:** Sysadmin
    - **Email:** jack.thomas@jmfsoft.com
    - **Phone:** (555) 123-4570
    - **Department:** IT
    - **Location:** San Francisco

11. **Kathy White**
    - **Username:** kwhite
    - **Occupation:** Customer Service Representative
    - **Email:** kathy.white@jmfsoft.com
    - **Phone:** (555) 123-4571
    - **Department:** Customer Service
    - **Location:** Houston

12. **Leo Harris**
    - **Username:** lharris
    - **Occupation:** Network Engineer
    - **Email:** leo.harris@jmfsoft.com
    - **Phone:** (555) 123-4572
    - **Department:** IT
    - **Location:** New York

13. **Mona Martin**
    - **Username:** mmartin
    - **Occupation:** Content Writer
    - **Email:** mona.martin@jmfsoft.com
    - **Phone:** (555) 123-4573
    - **Department:** Marketing
    - **Location:** Boston

14. **Nate Jackson**
    - **Username:** njackson
    - **Occupation:** Database Administrator
    - **Email:** nate.jackson@jmfsoft.com
    - **Phone:** (555) 123-4574
    - **Department:** IT
    - **Location:** Chicago

15. **Lara Vega**
    - **Username:** laravega
    - **Occupation:** Graphic Designer
    - **Email:** laravega@jmfsoft.com
    - **Phone:** (555) 123-4575
    - **Department:** Design
    - **Location:** Argentina

16. **Paul King**
    - **Username:** pking
    - **Occupation:** IT Support
    - **Email:** paul.king@jmfsoft.com
    - **Phone:** (555) 123-4576
    - **Department:** IT
    - **Location:** New York

17. **Quinn Scott**
    - **Username:** qscott
    - **Occupation:** Sales Representative
    - **Email:** quinn.scott@jmfsoft.com
    - **Phone:** (555) 123-4577
    - **Department:** Sales
    - **Location:** Miami
  
18. **Rachel Adams**
    - **Username:** radams
    - **Occupation:** Finance Analyst
    - **Email:** rachel.adams@jmfsoft.com
    - **Phone:** (555) 123-4578
    - **Department:** Finance
    - **Location:** Chicago

19. **Sam Turner**
    - **Username:** sturner
    - **Occupation:** DevOps Engineer
    - **Email:** sam.turner@jmfsoft.com
    - **Phone:** (555) 123-4579
    - **Department:** IT
    - **Location:** Seattle

20. **Tina Phillips**
    - **Username:** tphillips
    - **Occupation:** Office Manager
    - **Email:** tina.phillips@jmfsoft.com
    - **Phone:** (555) 123-4580
    - **Department:** Administration
    - **Location:** Los Angeles

Given the list of users, I will proceed to create 10 users with ADAC and 10 users with ADUC to practice both 'programs'. Accidental deletion protection will be activated in all cases. <br/><br/>
To simulate a 'real' work environment, I'm going to create a ticket in Jira Service Management (JSM) representing a hypothetical request for user creation. <br/><br/>

![Jira](images/jsm1.PNG)

In this case, I have already created a JSM (Jira Service Management) project previously. Now, I will create the ticket that simulates the experience of receiving it, 'working on it', and resolving it.

![Ticket](images/ticket.PNG) 
<br/> ..... 
<br/> ..... <br/>
![Ticket00](images/ticket00.PNG)

After simulating the ticket sent by Carlos Pérez (a user created by myself), I assign it to myself (indicating that I will handle the ticket) and start creating the users.

Finally, once the ticket is resolved (with all the users created), I get the following: 

![Users](images/users.PNG)

Therefore, once I have created all the requested users, I can mark the ticket as resolved and additionally leave a message for the user who initiated it, indicating that all the users have been successfully created. 

![Ticket2](images/ticket2.PNG)

### User creation through Azure Active Directory (currently Microsoft Entra ID)

In addition to managing Active Directory on-premises, you can use the cloud version, which is Azure Active Directory, currently known as Microsoft Entra ID.

![Azure1](images/azure1.PNG)

To do this, I must first have my domain created (in this case, I created the domain jmfsoft.onmicrosoft.com).

To create users, I go to the Users section and select New User -> Create New User:

![Azure2](images/azure2.PNG)
![Azure3](images/azure3.PNG)

Then, I fill in the desired fields.

![Azure4](images/azure4.PNG)

![Azure5](images/azure5.PNG)

Finally, I check that all the fields have been filled in correctly:

![Azure6](images/azure6.PNG)

If everything is correct, I press 'Create' to finish creating the user:

![Azure7](images/azure7.PNG)

### Bulk user creation with Azure Active Directory using a .csv file

To create multiple users at once, Azure Active Directory has a section that allows you to create multiple users based on an uploaded .csv file.

To do this, I go to Bulk operations -> Bulk Creation and download the corresponding .csv.

![Azure8](images/azure8.png)

![Azure9](images/azure9.PNG)

Once the .csv is downloaded, I fill in each column with the corresponding data, where each row will correspond to a particular user.

Once all users are created, I get the following:

![Azure11](images/azure11.PNG)

-------------------------------------------------------------------------------------------------

## Microsoft 365 Integration and Management

Microsoft 365 is a suite of cloud-based services and applications that provides organizations and individuals with tools for productivity, collaboration, and information management. It is an evolution of what was previously known as Office 365 and combines a variety of services for both team work and personal use. Here are some of the main tools and services it includes:

- **Office Applications**:

  - Word: Word processor.
  - Excel: Spreadsheet for data analysis.
  - PowerPoint: Tool for creating presentations.
  - Outlook: Email client and calendar.
  - OneNote: Application for note-taking and organizing information.

- **Cloud Services**:

  - OneDrive: Cloud storage for saving and sharing files.
  - SharePoint: Platform for content management and team collaboration, including the creation of websites and document libraries.

- **Collaboration and Communication**:

  - Teams: Communication and collaboration platform integrating chat, video calls, online meetings, and document collaboration.
  - Yammer: Enterprise social network for internal communication and employee collaboration.

- **Productivity and Management**:

  - Planner: Tool for task and project management.
  - To Do: Application for task management and to-do lists.
  - Power Automate: Service for automating workflows and repetitive tasks.
  - Power Apps: Platform for creating custom applications without deep programming knowledge.

- **Security and Administration**:

  - Microsoft Defender: Security tool for protecting against threats and malware.
  - Azure Active Directory (AAD): Service for identity and access management.
  - Intune: Mobile device and application management service to ensure devices comply with organizational policies.

Microsoft 365 offers an integrated, cloud-based approach to productivity and collaboration, streamlining the management of tools and services in a modern business environment.

Knowing Microsoft 365 is essential for anyone in a Help Desk position. In today’s business environment, many organizations rely on the applications and services offered by Microsoft 365 for their daily operations. These tools go beyond simple office applications, integrating advanced functionalities that enhance collaboration, productivity, and cloud information management.

### Acquire Microsoft 365 for the company

Steps to follow for Microsoft 365 setup:

1. Microsoft 365 can be obtained from the following link: [https://www.microsoft.com/es-es/microsoft-365/](https://www.microsoft.com/es-es/microsoft-365/)

    From that page, you can see the different plans available and the prices of each one.

    In this case, I will get an enterprise version, more specifically the following: Microsoft 365 Enterprise Standard.

    ![M365-1](images/m365-1.PNG)

2. To test this, I will use the monthly trial version.

    Then, I select the number of users I will have to manage. <br/>
    In this case, I indicate that I will use 25 users:

    ![M365-2](images/m365-2.PNG)

    Then, I select the user name, the company domain (in this case, JMFSoftCorp) and the password for that user: 

    ![M365-3](images/m365-3.PNG)

    Finally, I review the payment information, and confirm.

3. Once this is done, I go to the **Microsoft 365 Admin Center** and get the following interface:

    ![M365-4](images/m365-4.PNG)

    Now, I need to acquire the licenses within the menu called Marketplace:

    ![M365-5](images/m365-5.PNG)    

    Then, I choose the desired plan (in this case, Microsoft 365 Enterprise Standard) and correctly verify the number of licenses. In this case, I will opt for the option that says 'Trial' since I only want to use the trial version for practice purposes, but in a business environment, I would probably choose another option.
    
    ![M365-6](images/m365-6.PNG)
    
    Once everything is confirmed, I click on 'Start your free trial':

    ![M365-7](images/m365-7.PNG)

    And finally, I click on 'Place order'.

    Once this is done, the product and the 25 corresponding licenses are acquired, which will then be assigned to users according to their role in the company:

    ![M365-8](images/m365-8.PNG)


### Microsoft 365 User Creation

Steps to follow to create users in Microsoft 365:

1. First, I go to the Microsoft 365 Administration Center.

    Then, on the left panel, go to the Active Users section:

    ![M365-9](images/m365-9.PNG)

    Once inside, select the "Create user" option:

    ![M365-10](images/m365-10.PNG)

    Then, I fill in the fields, indicating first name, last name, display name (which will appear in the list of active users) and the user name, with which the user will log in:

    ![M365-11](images/m365-11.PNG)

    Once this is done, a very important step follows, which will be to assign the license to the user (in this case, Microsoft 365 Standard Enterprise) and the applications that can be used from it.

    For example, this is the section where the user will be assigned and allowed/blocked access to a certain program (e.g. Teams):

    ![M365-12](images/m365-12.PNG)

    Then, in the optional configuration section, you configure whether you want the user to be a "normal" user or an administrator user.

    If so, you will choose what type of administrator user it is, such as: Sharepoint Administrator, Exchange Administrator, Teams Administrator or Global Administrator if you want to give unlimited access to all features:

    ![M365-13](images/m365-13.PNG)

    Finally, it remains to confirm that all the data is correct and finish with the user creation.
    
    ![M365-14](images/m365-14.PNG)

    ![M365-15](images/m365-15.PNG)


### Microsoft 365 License Management

Steps to follow to manage Microsoft 365 licenses for a given user:

1. First, you must choose the user to whom certain licenses will be configured. <br/>
In this case I will choose to modify the licenses for the user “Lara Vega”, so I go to “Active users” and click on that user.

    Then, click on “Licenses and applications”:
    
    ![M365-16](images/m365-16.PNG)

2. Once I am inside this section, I have the licenses and applications available at my disposal.

    I can change the type of license (in this case no, because I only have a Microsoft 365 Standard Enterprise) and below the type of applications that the user can use (e.g. Teams, Sharepoint, etc).

    ![M365-17](images/m365-17.PNG)

    ![M365-18](images/m365-18.PNG)

3. Then, just select the licenses and applications and click on “Save changes”.
   

### Microsoft 365 Group Creation

Before going straight to the creation of groups, it is a good idea to define the different types of groups available in Microsoft 365 and the characteristics of each one.

* **Microsoft 365 Group**: A collaborative group that allows shared access to resources like Outlook, Teams, SharePoint, and more. It is fully compatible with all Microsoft 365 applications, is assigned an email address, and can integrate with Azure Active Directory for security management.

* **Distribution List**: Used to send emails to multiple recipients simultaneously. It is not compatible with Microsoft 365 applications and is only used for email distribution. It does not offer additional security features.

* **Security Group**: Designed to control access to resources within Microsoft 365 through roles and permissions. It is not compatible with Microsoft 365 applications and does not have an email address assigned. It offers strict access control to resources.

* **Mail-Enabled Security Group**: Similar to a security group but also allows sending emails to its members. It is not compatible with Microsoft 365 applications, but it is assigned an email address and offers strict access control to resources.
  
Once you understand the differences between each type of group, it is proper to continue with the steps to create them.

Steps to follow for the creation of groups in Microsoft 365:

1. First, I go to the Microsoft 365 Administration Center.

    Then, on the left panel, go to the Active groups and teams section:

    ![M365-19](images/m365-19.PNG)

2. Then, once inside the “Active groups and teams” panel, we first go to the section where the type of group we want to create is located (1) and then we choose the option “Create Microsoft 365 group / distribution list / security group” (2):

    ![M365-20](images/m365-20.PNG)

3. Then, we fill in the following data:

   - Group name and description
   - Owner/s of the group 
   - Group members 
   - Email (shared) of the group
   - If the group is public or private
   - If I want to connect Microsoft Teams to the group

    ![M365-21](images/m365-21.PNG)
    ![M365-22](images/m365-22.PNG)
    ![M365-23](images/m365-23.PNG)
    ![M365-24](images/m365-24.PNG)

    Once this is done, the group is successfully created:

    ![M365-25](images/m365-25.PNG)


### Microsoft 365 Shared Mailbox creation

Defining a shared mailbox in an organization provides a cost-effective way for multiple users to manage and collaborate on emails from a single, centralized address, ensuring consistent communication and streamlined workflows, while also offering access to shared calendars and contacts without the need for additional Microsoft 365 licenses.

Differences between a shared mailbox and a distribution list:

  ![SharedMailboxVsDistributionList](images/sharedmailbox01.PNG)

- **Distribution List**: A distribution list is a group of email addresses that acts as a forwarding mechanism. When an email is sent to the distribution list's address, it is automatically forwarded to all members of the list, but the list itself does not store emails. Unlike a shared mailbox, members cannot reply from the distribution list’s address; responses come from their individual email addresses.
  
- **Shared Mailbox**: A shared mailbox is an actual mailbox that multiple users can access to send, receive, and manage emails from a single, centralized address (e.g., support@company.com). Users can log in and interact with the mailbox, and emails sent from the shared mailbox appear to come from that shared address. It also includes shared calendars and contacts that all members can use.

A shared mailbox can be created from either the Microsoft 365 Admin Center or the Exhange Admin Center.
In this case, I will create it from the Microsoft 365 Admin Center.

Steps to follow to create a shared mailbox:

1. First, we go to the Microsoft 365 administration center, and then to the Teams and Groups section.

    Once there, we go to the “Shared Mailbox” section:

    ![SharedMailbox02](images/sharedmailbox02.PNG)

2. Then, click on “Add a shared mailbox” and fill in the fields with the necessary data (name and email address of the Shared Mailbox):
   
    ![SharedMailbox03](images/sharedmailbox03.PNG)

    Finally, we click on “Save Changes”.

3. Once the Shared Mailbox is created, I can click on it and define who the members of the Shared Mailbox are, and also what permissions each one has.

    ![SharedMailbox04](images/sharedmailbox04.PNG)

    In this case, I added 2 random members (Bob Smith and Carol Davis).

    In the permissions section, we find 3 options:

    1. **Read and Manage Permissions**: This permission, also known as "Full Access," allows a user to open the shared mailbox and view, read, delete, move, and organize emails, contacts, calendars, and other items within the mailbox. However, it does not allow the user to send emails from the shared mailbox unless additional send permissions are granted.
   
        Example: A user with Read and Manage permissions can log into the shared mailbox "support@company.com," review incoming support tickets, organize them into folders, delete spam, and archive old messages.

    2. **Send As Permissions**: This permission allows a user to send emails as the shared mailbox itself. When the user sends an email, it will appear as if the email was sent directly from the shared mailbox's email address, without indicating who specifically sent it.
   
        Example: A user with Send As permissions can send an email from "support@company.com," and the recipient will see the email as coming directly from "support@company.com," with no indication of the individual sender's name.

    3. **Send on Behalf Permissions**: This permission allows a user to send emails on behalf of the shared mailbox. The email will show the shared mailbox’s address, but it will also indicate the name of the individual who sent the email on behalf of the shared mailbox.

        Example: If a user has Send on Behalf permissions for "support@company.com" and sends an email, the recipient will see it as coming from "John Doe on behalf of support@company.com," clearly indicating who sent the message.


### Microsoft 365 File Recovery

Recovering files from OneDrive is crucial for a Help Desk Jr. because it allows them to quickly resolve common issues where users accidentally delete or lose important files, ensuring minimal disruption to productivity, improving user satisfaction, and demonstrating a strong understanding of cloud storage management in the organization.

In this case, to demonstrate file recovery, I will simulate that the user Lara Vega has created 2 files (Prueba and Prueba Excel) and accidentally deleted them.

![OneDrive](images/onedrive1.PNG)
 
Now, to simulate a real environment, I will create a ticket that simulates the request for help to restore those deleted files (Prueba & Prueba Excel):

![OneDrive2](images/onedrive2.PNG)

Once I have assigned the ticket to myself, and changed the status to “In progress”, I proceed to perform the following steps to resolve it:

1. First, I go to the Sharepoint Administration Center and then, I click on “More features” on the left panel:

    ![OneDrive3](images/onedrive3.PNG)

2. Once pressed, I go to the first section (User Profiles), and click on “Open”:

    ![OneDrive4](images/onedrive4.PNG)

3. Then, within the “User Profiles” menu, select the “Manage User Profiles” option:
   
    ![OneDrive5](images/onedrive5.PNG)

4. Now, within that section, I use the search engine to locate the user who deleted the files. 
In this case, the user is Lara Vega, so I proceed to locate her:

    ![OneDrive6](images/onedrive6.PNG)

    Once I have located the user, I use the dropdown that appears when I place the mouse on the “Account name” column, and choose the “Manage personal site” option:

    ![OneDrive7](images/onedrive7.PNG)

5. Finally, I select the recycle garbage can, but not before confirming that I am in the OneDrive of the desired user at the top left (Lara Vega's name appears): 
   
    ![OneDrive8](images/onedrive8.PNG)

6. Once all these steps have been followed, I have access to the recycle garbage can of the desired user, so I can simply select the files I want to restore and click on “Restore” (1):

    ![OneDrive9](images/onedrive9.PNG)

    If Lara Vega or someone else has deleted the files from the recycle bin, we will be able to recover them from the second level recycle bin (2), which "saves" us in these situations.

    If they have been deleted from the second level recycle bin, there is not much to do, but a lot of actions have to be performed for this to happen, so it is likely that such a scenario was intended.

<br/>

-------------------------------------------------------------------------------------------------

## Group Policy Management and Implementation

GPOs (Group Policy Objects) are a feature of Windows Server operating systems that allow administrators to centrally manage and configure policies and settings for systems and users in an Active Directory domain network. 

Basically, GPOs are collections of policy settings that can be applied to users and computers in an Active Directory-based network environment. These policies can cover a wide range of settings, from system security to user environment customization.

![GPO-1](images/gpo-1.PNG)

There are some basic concepts of GPOs that need to be understood before starting to configure them:

### GPO (Group Policy Objects) linking

GPOs must be linked to a container in Active Directory to be applied. 

Containers can be sites, domains or Organizational Units (OUs).

![GPO-3](images/gpo-3.PNG)

### GPO Inheritance / Precedence

GPO inheritance refers to how policies configured in GPOs are applied and inherited through the Active Directory hierarchy. 

The hierarchy follows this order:

1. **Local**: Local policies on the computer.
2. **Site**: Policies linked to a site in Active Directory.
3. **Domain**: Policies linked to the domain.
4. **Organizational Units (OUs)**: Policies linked to specific OUs.

Order of Application:

1. **Local**: Local policies are applied first.
2. **Site**: Site policies are applied after.
3. **Domain**: Domain policies are applied next.
4. **OUs**: OU policies are applied in order from the top-level OU to the most specific OU containing the object (user or equipment).

![GPO-2](images/gpo-2.PNG)

### Inheritance Modifiers

Mechanisms exist to modify how GPOs are inherited.

1. **Inheritance Blocking**:

    You can block inheritance of GPOs into a specific OU. This means that higher level policies will not be applied to that OU.
    
    How to enable: In the GPMC, select the OU, right click and select "Block Inheritance".

2. **Enforced**:

    A GPO can be marked as "Enforced", which means that its policies cannot be blocked by any lower OU.
    
    How to enable: In the GPMC, select the GPO, right click and select "Enforced".

3. **Security Filtering**:

    GPOs can be filtered to only apply to specific users or computers by using Security Groups.
    
    How to configure: In the GPMC, select the GPO, go to the "Scope" tab and adjust the permissions in the Security Filtering section.

Now that all the basic concepts are understood, it is time to show the steps for the application of such group policies.

### Steps to configure and apply a GPO

   1. **Open the Group Policy Management Console (GPMC)**:
   
      Log on to a domain controller or machine with the administrative tools installed (RSAT). <br/>
      Open the Group Policy Management Console (GPMC). You can do this by searching for "gpmc.msc" in the Start menu or through Administrative Tools.

      ![GPO-4](images/gpo-4.PNG)


   2. **Create a New GPO**:
   
      In the GPMC, navigate to the container in which you want to create the GPO. This can be a domain, site, or an organizational unit (OU). <br/>
      Right-click on the container and select "Create a GPO in this domain and link it here...". 

      In this case, I will create a GPO that applies to the entire domain. 

      ![GPO-5](images/gpo-5.png)

   3. **Name the GPO**:
   
      Assign a descriptive name to the new GPO. For example, "Password Policy" or "Desktop Wallpaper".  <br/>
      
      In this case, I will configure the GPO to apply a secure password policy. 

      ![GPO-6](images/gpo-6.PNG)

   4. **Edit the GPO**:
      
      Once created, the new GPO will appear in the list of GPOs linked to the selected domain, site or OU. <br/>
      
      Right-click on the GPO and select "Edit" to open the Group Policy Management Editor.

      ![GPO-7](images/gpo-7.png)

      Once this is done, the Group Policy Management Editor will open.

   5. **Configure Desired Policies**:
   
      In the Group Policy Management Editor, you can configure policies under two main categories:

        **Computer Settings**:

        - These policies are applied at the computer level. Examples include security settings, software installation, network policies, etc.

        **User Configuration**:

        - These policies are applied at the user level. Examples include desktop settings, folder redirection, software restrictions, etc.

      ![GPO-8](images/gpo-8.PNG)

      In this case, I go to:

        **Computer Configuration > Windows Settings > Security Settings > Account Policies > Password Policy**.

      ![GPO-9](images/gpo-9.PNG)

      Now, I double click on the policy to be modified and a window will open that will allow me to enable/disable this policy and also to modify the values.

      ![GPO-10](images/gpo-10.PNG)

      Finally, I click on "Apply" and the password length policy is set to 8 characters long.

      ![GPO-11](images/gpo-11.PNG)

   6. **Apply the GPO**:
   
      The GPO is already bound to the selected domain, site or OU, and the policies will be automatically applied to objects in that scope.

      The GPO will be processed on the next policy update cycle (typically every 90 minutes on computers and at user login). 

   7. **Force Policy Update**:

      To enforce policies immediately, you can force an update on affected computers and users:

        On the Domain Controller:

        - Run **gpupdate /force** at the command prompt to update policies on the domain controller.

        On Client Computers:

        - On each computer, **run gpupdate /force** at the command prompt to apply the new policies immediately. 

      ![GPO-12](images/gpo-12.PNG)


### Other examples of group policies that from my point of view would be good to apply

In addition to the password group policy that strengthens security, there are others that I believe are essential to implement in a business environment:

1. **Disable the use of USB devices**:

    **Policy**: Deny read and write access to removable storage devices.

    **Benefit**: Mitigates the risk of data loss and the introduction of malware through unauthorized USB devices.

    **Location**: Computer Configuration > Policies > Administrative Templates > System > Removable Storage Access

    ![GPO-13](images/gpo-13.PNG)

2. **Disable the installation of unauthorized software**:

    **Policy**: Disable installation of devices matching any of these device IDs.

    **Benefit**: Ensures that only approved software is installed, reducing the risk of system vulnerabilities and conflicts.

    **Location**: Computer Configuration > Policies > Administrative Templates > System > Driver Installation

    ![GPO-14](images/gpo-14.PNG)

3. **Disable access to the Control Panel and Settings**:

    **Policy**: Prohibit access to the Control Panel and Settings.

    **Benefit**: Prevents unauthorized changes to system settings and reduces the technical support burden due to inadvertent configuration.

    **Location**: User Configuration > Policies > Administrative Templates > Control Panel

    ![GPO-15](images/gpo-15.PNG)


### Example of what happens if I try to perform an action blocked by a group policy

If I try, for example, to access the control panel after it has been locked by group policy and group policies have been updated (either by their natural cycle or by the gpupdate /force command), I get the following message:

![GPO-16](images/gpo-16.PNG)

-------------------------------------------------------------------------------------------------

## Password Reset

Now, to continue with the homelab, I will practice another fundamental skill, which is resetting user passwords. Again, I will create a ticket from the same person to simulate a real environment.

Once the ticket is created and the request is submitted, I log in as 'IT Support' and view the ticket in question:

![Ticket-Reset](images/ticket-reset.PNG)

To do this, I go to **Active Directory Users and Computers (ADUC)** or **Active Directory Administration Center (ADAC)**, and locate the user.

### Reset password on Active Directory Users and Computers (ADUC)

In this case, the search can be done through the organizational units (as marked with 1, which gave me the results of the design department) or through the search function, useful when we have many users (as marked with 2):

![Ticket-Reset2](images/ticket-reset2.png)

Once I have located the user, I right click on it and 'Reset password':

![Ticket-Reset3](images/ticket-reset3.png)

Now, I must enter a temporary password that will be given to the user to try to log in next time.

It is important to check the box that indicates that the user will have to change the password at the next login, this is a good practice and its use is recommended.

Finally, it gives us the possibility to unblock the user, although in this case the user did not actually block the account.

![Ticket-Reset4](images/ticket-reset4.png)

Once this is done, I get a confirmation message:

![Ticket-Reset5](images/ticket-reset5.png)

### Reset password on Active Directory Administrative Center (ADAC)

Resetting a password in the Active Directory Administrative Center is just as easy, in addition to the fact that it has a more user-friendly interface.

Once ADAC is open, we can locate a section where we can easily reset a user's password:

![Ticket-Reset6](images/ticket-reset6.png)

If I do not opt for this method, I can simply move around the domain until I locate the corresponding organizational unit (OU) and then locate the user:

![Ticket-Reset7](images/ticket-reset7.png)

Now, once I have located the user, I can choose to right click on it and select 'Reset password' (1) or simply use the right sidebar (2) which also has this option:

![Ticket-Reset8](images/ticket-reset8.png)

Again, I fill in the field with the temporary password and select the option for the user to change it at the next login:

![Ticket-Reset9](images/ticket-reset9.png)

Done! Once this is completed, the user must log in with the temporary password and then will be prompted to update it. 

### Reset password on Azure Active Directory

To reset a password in Azure Active Directory, I go to the users section.

![Ticket-Reset10](images/ticket-reset10.png)

Then, I select the user to reset the password.

In this case, as the ticket indicated, the user is “Lara Vega”:

![Ticket-Reset11](images/ticket-reset11.png)

Once in the user's profile, all you need to do is click on “Reset password” at the top and a sidebar will open:

![Ticket-Reset14](images/ticket-reset14.png)

![Ticket-Reset12](images/ticket-reset12.png)

Once the button is pressed, the user's password will be reset and a temporary password will be automatically generated, which must be given to the user so that he/she can change it later. 

In this case, the temporary password is “Xavo6347”.

![Ticket-Reset13](images/ticket-reset13.png)

-------------------------------------------------------------------------------------------------

## Multi-factor authentication (MFA)

Multi-Factor Authentication (MFA) is a crucial security measure that adds an additional layer of protection to digital systems and accounts. Its main purpose is to mitigate the risks associated with password-only authentication, which can be vulnerable to various forms of attacks such as phishing, password theft, and the use of weak passwords.

![MFA-1](images/mfa1.png)

Enabling multi-factor authentication in an enterprise environment is a best practice because it significantly enhances security, reduces the risk of account compromise and meets regulatory standards, all while improving access management and protecting the organization's critical data from both internal and external threats.

First, I am going to simulate the creation of a ticket requesting the activation of this feature (Multifactor Authentication).

![MFA-3](images/mfa3.PNG)

To do this, I perform the following steps:

1. First, I go to the Azure portal --> https://portal.azure.com/
   
    ![MFA-2](images/mfa2.PNG)

2. Then, I go to the "Users" tab and click on the "MFA per user":
   
   ![MFA-4](images/mfa4.PNG)

3. Once clicked, I get the following interface:
   
   ![MFA-5](images/mfa5.PNG)

4. Now, I must choose which users I want to enable multi-factor authentication.
  In this case, the ticket states that I want to enable it for all users, so I select all users and then click on "Enable":

    ![MFA-6](images/mfa6.PNG)
   
5. Once I click on "enable", the following dialog box is displayed:
   
   ![MFA-7](images/mfa7.PNG)

6. Once accepted, the following banner is displayed, indicating that multi-factor authentication has been enabled for the selected users:
   
   ![MFA-8](images/mfa8.PNG)

7. In case I am not sure, I can check that MFA was enabled correctly by seeing the word "Enabled" in the section " MULTI-FACTOR AUTH STATUS":
   
   ![MFA-9](images/mfa9.PNG)

Finally, it remains to answer the ticket indicating to the user (in this case, Carlos Perez), that the multifactor authentication (MFA) was successfully enabled.

In addition, evidence is attached to the ticket demonstrating the indicated action.

![MFA-10](images/mfa10.PNG)

-------------------------------------------------------------------------------------------------

## Software Deployment

In this section, I will use a software deployment program, **PDQ Deploy**, to simplify and automate the installation and updating of applications across multiple machines in my homelab. The reasons for implementing a software deployment tool like PDQ Deploy are:

- **Efficiency and Time-Saving**: Manually installing or updating software on each client machine can be very time-consuming, especially in a larger environment. PDQ Deploy streamlines this process by allowing for the automated deployment of software packages across all connected devices, saving significant time and effort.

- **Consistency and Control**: Using PDQ Deploy ensures that all machines receive the same software version and configuration, reducing the risk of discrepancies and compatibility issues. This helps maintain a uniform and controlled IT environment.

- **Centralized Management**: PDQ Deploy provides a centralized interface to manage software installations, making it easier to track deployment status, manage software versions, and perform updates from a single point of control.

- **Scalability**: As my homelab grows, manually handling software installations becomes increasingly impractical. PDQ Deploy scales with the environment, allowing for efficient software management regardless of the number of machines involved.

In this case, I will do it from the secondary server (SV02) to balance the load and not overburden the domain controller (DC01).

This tool can be downloaded from the following website: [PDQ Deploy](https://www.pdq.com/pdq-deploy/)

Now, I proceed to download and install it on the secondary server (SV02).

To do this, I need to create an account, which will give me a free trial of the full program for 14 days, although the basic functionality (software deployment) can still be used after those 14 days:

![PDQ](images/pdq1.PNG)

Once registered, I am redirected to the following page where I get the links to download PDQ Deploy & PDQ Inventory, along with their licenses (which last for 14 days).

![PDQ2](images/pdq2.PNG)

After installing (as a server, since SV02 will be in charge of deploying to client machines), the program interface will look like this:

![PDQ3](images/pdq3.PNG)

Now, to begin the deployment, first, I need to have the installer of the software I want to deploy downloaded and saved in a location.

In this case, for simplicity, I'll choose to install 7zip.

The installer will be located in a folder on the desktop named 'Software to Deploy':

![PDQ4](images/pdq4.PNG)

Now, I open PDQ Deploy and select 'New Package'. Then, I fill in the fields.

![PDQ5](images/pdq5.PNG)
![PDQ6](images/pdq6.PNG)

Afterwards, I go to 'Steps' and then select 'Install':

![PDQ7](images/pdq7.PNG)

Then, I select the installer, add the parameters for silent installation (in this case, the appropriate one for 7zip is /S) to avoid disturbing any user while they are working, and press save.

![PDQ8](images/pdq8.PNG)

Once this is done, the package will appear already created. Now, all that's left is to right-click on it and press 'Deploy Once' to begin the deployment.

![PDQ9](images/pdq9.png)

Finally, I must select the users/computers by clicking on 'Choose Targets', and that's it, I can start the deployment:

![PDQ10](images/pdq10.PNG)

In this case, just for testing purposes, I'll select client machines 1 and 2 (JMFSOFT-PC01 and JMFSOFT-PC02):

![PDQ11](images/pdq11.PNG)

After initiating the deployment, I get the following results:

![PDQ12](images/pdq12.PNG)

Now, I verify that 7zip was installed correctly on the client machines:

![PDQ13](images/pdq13.PNG)

And, with this, I can successfully conclude the deployment.

-------------------------------------------------------------------------------------------------

## Remote Access and Troubleshooting

In this section of the project, remote access between company computers will be implemented for troubleshooting purposes.

This concept is fundamental in day-to-day IT support, since it is a required skill that is very useful because at times it may not be possible to physically access a computer (due to geographical issues, for example).

There are many tools on the market that facilitate remote access, such as TeamViewer or AnyDesk.
Even Windows comes with a tool built into the operating system itself, but for reasons of versatility I will use TeamViewer because it works on many operating systems and it is also my tool of choice, but I could easily use AnyDesk, which also has a very simple interface, less resource consumption and lower licensing costs.

![Teamviewer-Logo](images/teamviewer-logo2.jpg)

In this case, the connection will be made from the SV02 machine to the JMFSOFT-PC01 client machine.

To do this, I must perform the following steps:

1. First, I need to install TeamViewer on both computers.

    To download the executable I go to [https://www.teamviewer.com/](https://www.teamviewer.com/) and click on Free Download.

    ![TeamViewer-1](images/tw1.PNG)

2. Once this is done, I select the desired plan. 
    
    In this case, I click on "Free", but in a business environment you may have to opt for the other licensing options.

    ![TeamViewer-2](images/tw2.PNG)

    Now, I must choose which type of installation I want, each one with its respective functionalities and/or advantages as appropriate.

    Among the options are:

    - **TeamViewer QuickSupport**: No full installation is required; it is a lightweight, portable application. Ideal for quick and occasional support.

    - **TeamViewer Full Client**: Offers all TeamViewer functionalities for full remote access and control. Allows both inbound and outbound connection, which means you can control other devices or be controlled. Includes features such as file transfer, chat, and Wake-on-LAN.

    - **TeamViewer Host**: Provides continuous remote access to devices, ideal for long-term remote management. Requires installation on the device. Allows unlimited and permanent remote connections. Ideal for keeping computers and servers under constant surveillance and support.

    - **TeamViewer MSI Package**: Facilitates the distribution and management of TeamViewer installations through group policies in corporate networks. MSI format suitable for deploying and managing installation on multiple devices via Active Directory and GPOs. Supports custom configurations to suit specific needs. Allows automating installation and configuring pre-deployment settings.

    - **TeamViewer Meeting**: Dedicated platform for online meetings, video conferencing, and real-time collaboration. Provides tools for high-quality audio and visual communication. Includes screen sharing, chat, and meeting recording functionalities.

    Below is a comparative table showing the main features between each version:

    ![TeamViewer-4](images/tw4.PNG)

    In this case I chose to install the "Full Client" version on the SV02, in its 64-Bits version.

3. Once downloaded and installed, the client is executed, which looks as follows:

    ![TeamViewer-5](images/tw5.PNG)

    To make a connection, it is necessary to use the ID and password.

    These credentials should only be shared between the IT Support Technician and the person who wishes to receive help from the IT Support Technician, otherwise anyone could gain remote access to the company's equipment.

4. Now, I download and install the QuickSupport version on the JMFSOFT-PC01 computer (just for demonstration and simplicity, as I could have opted for the Full Client version).

    ![TeamViewer-6](images/tw6.PNG)

5. Finally, just share the credentials of the client computer (JMFSOFT-PC01) with the IT Support manager so that he/she can enter them in the TeamViewer client to gain remote access and troubleshoot problems.
   
-------------------------------------------------------------------------------------------------

## VPN Configuration

In addition to using remote access to troubleshoot problems, a good practice is to use remote access via VPN.

A **Virtual Private Network (VPN)** is a technology that creates a secure, encrypted network connection over a public network, such as the Internet. It allows users to send and receive data securely over shared or public networks as if their devices were connected directly to a private network.

### Why is it considered a good practice?

- **Data Security:**

  - **Encryption:** VPNs use encryption protocols to protect the data transmitted between the user and the VPN server. This makes the information unreadable to anyone intercepting the connection.
  
  - **Authentication:** VPNs may require authentication, which ensures that only authorized users can access the network.
  
- **Privacy:**
  
  - **IP Hiding:** The VPN hides the user's IP address, which helps maintain online privacy by making user activity less traceable.
  
  - **Identity Protection:** By hiding the user's location and identity, VPNs protect against tracking and data collection by third parties.

- **Secure Remote Access:**
  
  - **Remote Work:** VPNs allow employees to securely access the corporate network from remote locations, which is crucial for teleworking and workforce mobility.
  
  - **Corporate Resources:** Users can access internal corporate resources, such as files, applications and databases, as if they were physically in the office.

- **Censorship and Geographical Restrictions Avoidance:**

  - **Accessing Blocked Content:** VPNs allow users to access websites and services that may be blocked or restricted in their geographic location.
  
  - **Censorship Avoidance:** They can be used to circumvent censorship in regions where Internet access is controlled or restricted.

- **Information Integrity:**

  - **Protection Against Tampering:** VPNs' encryption and security protocols protect data from alteration and tampering while in transit.


### Conclusion

Implementing VPNs is a best practice for its ability to provide a secure and private connection over public networks, protect sensitive information and allow secure remote access to network resources. This not only improves overall network security, but also ensures the integrity and privacy of transmitted data.

### Steps to set up a VPN

1. First, I must choose on which server I want to install the role.

    In this case I will choose to install the Remote Access role on the SV02 server.
    This helps distribute the workload and improves security by not directly exposing the domain controller to external connections.

    To start, I open the Server Manager:

    ![VPN-1](images/vpn1.PNG)
 
2. Then, to add the role, I go to “Manage” and then “Add roles and features”:

    ![VPN-2](images/vpn2.PNG)

3. Now, I select the “Remote Access” role and install it:
   
   ![VPN-3](images/vpn3.PNG)

4. Then, I select the “DirectAccess and VPN (RAS)” option:

    ![VPN-4](images/vpn4.PNG)

    * **DirectAccess** is a remote access technology introduced in Windows Server 2008 R2. It allows remote users to access internal network resources without the need to manually initiate a VPN connection. DirectAccess automatically establishes a secure, persistent connection between the user's device and the corporate network, providing an experience similar to being in the office.
     
    * **VPN** is a technology that creates a secure, encrypted connection over a less secure network, such as the Internet. VPN allows remote users to access internal network resources as if they were directly connected to the network. 

5. Finally, I review the selected items and confirm that everything is correct to start with the installation of the Remote Access role, so I press “Install”:

    ![VPN-5](images/vpn5.PNG)

6. Once the installation is complete, a message will be displayed indicating that the installation was successful, but that configuration is required:

    ![VPN-6](images/vpn6.PNG)

7. Now, it is time to configure the Virtual Private Network (VPN), which will allow remote access to employees in a distant geographical location.

    To do this, I must open the “Introduction Wizard”, which is displayed when I finish installing the “Remote Access” role:

    ![VPN-7](images/vpn7.PNG)

8. Once the wizard is opened, a window containing 3 options will be displayed:

    ![VPN-8](images/vpn8.PNG)

    * **Implement DirectAccess and VPN (Recommended)**: Implementing both VPN and DirectAccess offers flexibility, allowing users to choose between automatic (DirectAccess) or manual (VPN) connections based on their needs. This option provides compatibility across various devices and operating systems that do not support DirectAccess, such as macOS and Linux. DirectAccess ensures a seamless and continuous connection to the corporate network, ideal for users who require uninterrupted access. It also enhances security by utilizing multiple authentication and encryption methods. However, managing both technologies can be complex, requiring more time and resources, and DirectAccess requires IPv6, which might necessitate additional network configurations.

    * **Implement DirectAccess only**: Implementing DirectAccess Only provides a transparent connection experience, as users do not need to manually initiate the connection. It allows continuous remote management of devices, even when users are not logged in, and uses IPsec for secure communication with the option for multi-factor authentication. The downside is its limited compatibility, as it only works with Windows devices that support DirectAccess, and it requires IPv6 and additional network configurations.
  
    * **Implement VPN only**: Implementing VPN Only offers broad compatibility with various devices and operating systems, including Windows, macOS, Linux, Android, and iOS. It is generally simpler and more straightforward to configure compared to DirectAccess and allows for flexibility in choosing VPN protocols based on security and compatibility needs. However, users must manually initiate the VPN connection each time they need access to the corporate network, and administrators typically cannot manage remote devices when they are not connected to the VPN.
 
    In this case, I will select the option “**Implement VPN only**”.

9. Once this option is selected, a new window called “Routing and Remote Access” will open.
    
    ![VPN-9](images/vpn9.PNG)

    In this case, you can see by the icon with the red arrow that the service is not currently configured and enabled. 

    To configure it, we right click on it and select the option “Configure and enable Remote Access”:

    ![VPN-10](images/vpn10.png)

10. Once the wizard is opened, a configuration menu opens where I have to choose a combination of services to provide:

    - **Remote Access:** Configures the server to function as a Remote Access Service (RAS), allowing users to connect to the corporate network via dial-up lines or private networks.
  
    - **Network Address Translation (NAT):** Configures the server to act as a NAT device, enabling devices on a private network to access the Internet using a single public IP address. NAT hides internal IP addresses, enhancing security and conserving public IP addresses.
  
    - **Virtual Private Network (VPN) and NAT access:** Configures the server to provide both VPN and NAT services. This allows remote users to securely connect to the corporate network over the Internet (VPN) and also provides NAT services to manage network traffic between internal and external networks.
  
    - **Secure connection between two private networks:** Sets up a secure connection between two distinct networks, allowing them to communicate securely over the Internet. This is ideal for connecting different branches of a company, creating a private virtual network between them.
  
    - **Custom configuration:** Allows for detailed configuration of remote access services. You can manually select the specific services you want to configure, such as VPN, NAT, routing, dial-up remote access, etc. This option offers the most flexibility to tailor the setup to specific needs.

    In this case, I select the first option (Remote Access), since I am only interested in providing the VPN service:

    ![VPN-11](images/vpn11.PNG)

    Then, I select the “VPN” option:

    ![VPN-12](images/vpn12.PNG)

    Now, I must select the interface that connects the server to the Internet. 

    In this case, it is the “Ethernet” interface, whose IP address is 192.168.1.6:

    ![VPN-13](images/vpn13.PNG)

    Now, I must select whether I want the IP addresses of the computers connecting to the VPN to be assigned automatically (via DHCP) or whether I want an IP address to be chosen from a defined range.

    In this case, I will select the option “From a specified address range”:

    ![VPN-14](images/vpn14.PNG)

    Next, I create the range of addresses that I will choose to be assigned to those computers that connect to VPN.

    As this is an example case, I will select an agreed range that will only cover 8 IP addresses, from 192.168.1.88 to 192.168.1.95:

    ![VPN-14-2](images/vpn14-2.PNG)

    Finally, I check that everything is correct and press “Finish”:

    ![VPN-16](images/vpn16.PNG)

    Once this is done, the VPN server will be running: 

    ![VPN-17](images/vpn17.PNG)

11. Now, to connect to a VPN from a client computer (in this case, JMFSOFT-PC04), I go to the search bar and type VPN.

    Then, I select the “VPN Configuration” option:

    ![VPN-CLI1](images/vpncli1.PNG)

    Then, I select the “Add a VPN connection” option:

    ![VPN-CLI2](images/vpncli2.PNG)

    Then, I fill in the following fields:

    ![VPN-CLI3](images/vpncli3.PNG)
    ![VPN-CLI4](images/vpncli4.PNG)

    Once the fields have been filled in, the connection will be saved.

    All you have to do is click on connect:

    ![VPN-CLI5](images/vpncli5.PNG)
    ![VPN-CLI6](images/vpncli6.PNG)

    As shown in the image, the connection was successful.


12. Now, I can verify this as follows:

    From the Client side:

    * I go to CMD or Powershell and type ipconfig /all, and the connection should appear:

      ![VPN-CLI7](images/vpncli7.PNG)

    From the VPN server side:

    * I go to the “Routing and Remote Access” console, and then to the “Remote Access Clients” section.

      This section should list all the client computers connected to the VPN:

      ![VPN-CLI8](images/vpncli8.PNG)

This concludes the VPN server configuration.
  
-------------------------------------------------------------------------------------------------

## Monitoring and Visualization

In this section, I will install and integrate two programs, **Zabbix and Grafana**, to monitor client machines. This setup will enable real-time tracking and management of system performance and network activity. The primary reasons for including Zabbix, a robust monitoring tool, and Grafana, a powerful real-time data visualization program, in my homelab are:

- **Proactive Issue Detection**: Zabbix will help in identifying and addressing potential issues, such as hardware failures, software crashes, or network disruptions, before they impact the system's functionality. Grafana will provide a clear, visual representation of these issues, making them easier to understand and act upon.

- **Performance Optimization**: Continuous monitoring with Zabbix and real-time data analysis through Grafana will allow for fine-tuning system performance and ensuring efficient resource utilization.

- **Security Enhancement**: By using Zabbix to keep a close watch on system activities and Grafana to visualize security metrics, I can quickly detect any unauthorized access or unusual behavior, thereby strengthening the security of the environment.

- **Learning and Experimentation**: Implementing Zabbix and Grafana will provide hands-on experience in setting up, configuring, and utilizing monitoring and data visualization technologies, which are critical skills for IT management and cybersecurity.

![Zabbix Logo](images/zabbix-logo.png)![Grafana Logo](images/grafana-logo.jpg)

To monitor the machines, I will use the Linux operating system, specifically the Debian 12.5 distro, on which I will install Zabbix and Grafana, tools for monitoring and data visualization respectively.

To start creating the Linux virtual machine, I first need to download the Debian distro, which can be downloaded from the official website: [debian.org](https://www.debian.org/index.es.html).

Once downloaded, I proceed to start creating the virtual machine:

First, I specify the name of the machine, which will be **MON01** as it is an abbreviation for 'Monitoring', clearly indicating that the machine is dedicated to monitoring.

Additionally, I check the box to place the virtual machine in a location different from the default setting in Hyper-V. In this case, I chose to store it in **C:\Hyper-V\VMs\\**, the same location where I stored all the company's machines.

![Linux 1](images/lnx1.PNG)

In this case, I will choose to allocate **3GB (3072MB)** of RAM to the machine. Considering that 2048MB (2GB) is a basic amount for a small network of up to 10 devices and 4096MB (4GB) is a moderate amount for a network of up to 30-50 devices, I opted for an intermediate value between the two. This should not pose any issues, as it can be easily configured in Hyper-V.

![Linux 2](images/lnx2.PNG)

Next, in the disk configuration, I choose to create a new virtual hard disk (VHD) with a size of 2TB (2048GB). This size is more than sufficient (even excessive) but ensures that I won't encounter any storage issues on the virtual machine.

![Linux 3](images/lnx3.PNG)

Next, I select 'Install an operating system from a bootable CD/DVD-ROM' and locate the downloaded .ISO file of the Debian operating system, which in my case is located at **C:\Users\juanm\OneDrive\Desktop\Herramientas\Sistemas Operativos\\**.

![Linux 4](images/lnx4.PNG)

Finally, I review all the settings and verify that everything is correct. As it is the case, I press 'Finish' to complete the virtual machine creation.

![Linux 5](images/lnx5.PNG)

As you can see in the image, the virtual machine was created correctly. Now, I proceed to start it to begin with the installation of the operating system.

![Linux 6](images/lnx6.PNG)

Once the virtual machine is started, the Debian 12.5 operating system will begin to be installed.
First, I must indicate what action I want to perform. 
In this case, I will perform a 'Graphical Install'.

![Linux 7](images/lnx7.PNG)

Now, I must select the language.
In this case, I select "English", but I could have selected my native language (Spanish).

![Linux 8](images/lnx8.PNG)

The installer will then load the installation components from the downloaded .ISO file. <br/> After that, it will try to obtain an IP address via DHCP (Dynamic Host Configuration Protocol).

Once all this happens, I must define the hostname. 
In this case, I will choose to put "deb-mon01", which is a mix between the operating system (debian) and the main function of the machine (monitoring).

![Linux 9](images/lnx9.PNG)

Now, I must define the password of the root user, which is the most important user of the system. For this reason, this password cannot be forgotten.

![Linux 10](images/lnx10.PNG)

Now, I select a user name which will be created to perform the tasks that do not require elevated privileges. This is similar to creating a local user in Windows in addition to the Administrator user.
In this case, I will choose to create the user "juanma".

![Linux 11](images/lnx11.PNG)

Now, it is time to partition the disks.
In this case, I chose a guided partition as I find no justification for partitioning manually on a homelab.
In a real environment, this should be configured in a more detailed way, creating a partition for the operating system and one for the swap.

![Linux 12](images/lnx12.PNG)

Finally, a summary of the automatic partitioning is shown.
Although the "swap" partition is created, which serves as an extension of the virtual memory when the physical RAM (Random Access Memory) is fully utilized, the amount allocated may not be the recommended amount, but for a test environment such as this homelab this is more than enough.

![Linux 13](images/lnx13.PNG)

After unpacking and installing the base files, the system will query if a proxy is being used (in this case no) and which is the country from where I am using the operating system to recommend mirrors from where the desired packages will be installed.

Next, the system starts configuring **'apt' (Advanced Package Tool)**, which is a command line tool used to manage software packages. Its main function is to facilitate the installation, update and removal of applications and utilities in the operating system.

![Linux 14](images/lnx14.PNG)

Then, it is asked about what software you want to install.
In this case, several desktop environments are shown (I will choose to use GNOME), the web server and the SSH Server (which serves to allow remote connections to the machine).
In this case, I won't install either (web or ssh).

![Linux 15](images/lnx15.PNG)

Finally, GRUB (the operating system boot loader) is installed on the primary partition and then the installation is completed.

![Linux 16](images/lnx16.PNG)

Once the computer has restarted, I find the following screen in which I can log in to the system with the user (juanma) and the previously defined password:

![Linux 17](images/lnx17.PNG)

Once this is done (logged in), I am already using the Debian operating system.

![Linux 18](images/lnx18.PNG)

Following this, I must install Zabbix and Grafana to begin monitoring the company's equipment.

### Zabbix Installation

To begin the installation of Zabbix, I go to their official website --> [https://www.zabbix.com/download](https://www.zabbix.com/download) and select the following options:

![Zabbix 1](images/zbx1.PNG)

**ZABBIX VERSION**: 6.4, the latest stable version <br/>
**OS DISTRIBUTION**: Debian <br/>
**OS VERSION**: 12 (Bookworm) <br/>
**ZABBIX COMPONENT**: Server, Frontend, Agent (since I want to install the Zabbix server on this machine to monitor other client devices) <br/>
**DATABASE**: MySQL (in this case, I choose MySQL because its configuration and management are simpler. I don't need to handle a large volume of data writes and I prioritize performance for read queries. However, in an enterprise environment, PostgreSQL might be more useful if complex transaction processing is needed) <br/>
**WEB SERVER**: Apache (I choose Apache because it has native compatibility with Zabbix, is easier to configure, and I don't require handling many concurrent connections. This doesn’t justify installing Nginx, which excels at efficient management of concurrent connections and scalability) <br/>

Once all these options are selected, I need to follow the installation manual on the website, which is tailored to the previously chosen options. <br/>
To run the following commands, you need to open a terminal, which allows you to interact with the operating system using text-based commands.

![Zabbix 2](images/zbx2.PNG)

The first command (**wget**) downloads the Zabbix 6.4 .deb package, which is then installed with the next command (**dpkg -i**). <br/>
Finally, **apt update** is run, which updates the list of available packages and their versions from the configured repositories. This ensures that the system has the most up-to-date information about the packages available for installation or upgrade.

![Zabbix 3](images/zbx3.PNG)

![Zabbix 4](images/zbx4.PNG)

![Zabbix 5](images/zbx5.PNG)

Once these commands are executed, we proceed to install the Zabbix server, frontend, and agent:

![Zabbix 6](images/zbx6.PNG)

After executing the command, I will be prompted to confirm the installation of the listed packages. To confirm, type 'y' (for yes) and press enter. Alternatively, I could skip this prompt by adding the -y parameter to the original command.

![Zabbix 7](images/zbx7.PNG)

Once this is done, I proceed to create the initial database in MySQL:

![Zabbix 8](images/zbx8.PNG)

In this case, Debian 12.5 uses MariaDB (a MySQL fork compatible with most applications that use MySQL) instead of MySQL.
Because of this, I will use the equivalent commands for the MariaDB database.

![Zabbix 9](images/zbx9.PNG)

The commands executed in the image perform the following actions:

- **sudo mariadb -u root -p**: This command initiates an interactive session with the MariaDB database server as the root user. The -p parameter prompts for the password of the root user to authenticate.

- **create database zabbix character set utf8mb4 collate utf8mb4_bin;**: Creates a new database named zabbix on the MariaDB server. utf8mb4 specifies the UTF-8 character set for full Unicode support, capable of storing a wide range of characters. utf8mb4_bin specifies the binary collation setting, which is useful for binary string comparison without regard to specific regional settings.

- **create user 'zabbix'@'localhost' identified by 'Test.123';** Creates a new user in the MariaDB database named zabbix, restricted to connections from the same server (localhost). The password for this user is set to 'Test.123'.

- **grant all privileges on zabbix.\* to 'zabbix'@'localhost';**: Grants all privileges on the zabbix database to the zabbix user when connecting from localhost. This allows the zabbix user to perform all operations (create, modify, delete, select, etc.) on all tables and objects within the zabbix database.
  
- **set global log_bin_trust_function_creators = 1;**: Enables the safe execution of user-defined functions (UDF) in a binary log replication environment. This is necessary to allow Zabbix, which often uses user-defined functions, to function correctly in database replication environments.

![Zabbix 10](images/zbx10.PNG)

![Zabbix 11](images/zbx11.PNG)

The command "**zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | sudo mariadb -u zabbix -p zabbix --default-character-set=utf8mb4**" is used to import a gzip-compressed database file into MariaDB, specifically for the Zabbix monitoring system.

In this command, the file "/usr/share/zabbix-sql-scripts/mysql/server.sql.gz" is decompressed using zcat and then imported into MariaDB. The command is prefixed with "sudo" to execute it with superuser privileges, ensuring sufficient permissions to perform the import. "-u zabbix" specifies that the user "zabbix" will be used to connect to MariaDB, and "-p zabbix" indicates that the password for the user "zabbix" will be used. The option "--default-character-set=utf8mb4" ensures that UTF-8 character set encoding is used.

![Zabbix 12](images/zbx12.PNG)

![Zabbix 13](images/zbx13.PNG)

Executing the command "**SET GLOBAL log_bin_trust_function_creators = 0;**" sets the parameter to 0. This parameter controls whether non-superuser accounts can create user-defined functions when binary replication (log_bin) is enabled. Setting it to 0 restricts this capability, ensuring tighter control over the functions created in the database.

After performing these steps, it is necessary to configure the database for the Zabbix server:

![Zabbix 14](images/zbx14.PNG)

![Zabbix 15](images/zbx15.PNG)

Now that everything is configured, I need to restart the 'zabbix-server', 'zabbix-agent', and 'apache2' services using **systemctl restart**. Additionally, I will enable them to start automatically at system boot using **systemctl enable**.

![Zabbix 16](images/zbx16.PNG)

![Zabbix 17](images/zbx17.PNG)

Once the services are restarted and enabled, I proceed to open the Zabbix web interface (Zabbix UI) by accessing the link http://[host]/zabbix.
In this case, the hostname is: deb-mon01, but if I don't know it, I can run the command 'hostname'.

![Zabbix 18](images/zbx18.PNG)

![Zabbix 19](images/zbx19.PNG)

![Zabbix 20](images/zbx20.PNG)

Once the language is selected, I click 'Next Step' and verify that all prerequisites are met (in this case, as shown in the image, all are satisfactorily met):

![Zabbix 21](images/zbx21.PNG)

Now, I configure the database connection.
In this case, I just need to verify that the username is correct (zabbix), and I must fill in the password with the one used previously (Test.123):

![Zabbix 22](images/zbx22.PNG)

Now, I need to fill in the Zabbix web server name, set the time zone, and choose the web interface theme. In this case, I opted for the name "Zabbix Server - Buenos Aires Office" to identify the Zabbix server. It's a clear and descriptive name indicating the purpose and location of the server.

The time zone is (UTC-03:00) America/Argentina/Buenos Aires, and the chosen theme is 'Blue'.

![Zabbix 23](images/zbx23.PNG)

Finally, a summary of the selected configurations is displayed. I verify that everything is correct and then click 'Next Step'.

![Zabbix 24](images/zbx24.PNG)

Then, a message appears indicating that the Zabbix server has been successfully configured and installed.

![Zabbix 25](images/zbx25.PNG)

Once 'Finish' is pressed, I am redirected to a login where I must enter the following credentials:

**Username**: Admin <br/>
**Password**: zabbix

![Zabbix 26](images/zbx26.PNG)

Once this is done, I conclude with the configuration and installation of the Zabbix server. Now, the only thing left is to install the client on the machines I want to monitor or discover them automatically on the network.

![Zabbix 27](images/zbx27.PNG)

Once installed on Linux, I proceed to download the agent on Windows. <br/>
In this case, I will install and configure the agent manually on each machine, but I could do it using PDQ Deploy from SV02.

![Zabbix 28](images/zbx28.PNG)

Once downloaded and started, I must indicate the IP address of the Zabbix server (i.e. the Debian 12.5 machine where I installed the Zabbix server).
In this case, I can verify this information using the ip address show command (or ip a s for short).

![Zabbix 29](images/zbx29.PNG)

![Zabbix 30](images/zbx30.PNG)

Then, I press 'Next' and finish the agent installation. After that, I proceed to repeat this process on the other machines (DC01, JMFSOFT-PC01, JMFSOFT-PC02, JMFSOFT-PC03, JMFSOFT-PC04).

![Zabbix 31](images/zbx31.PNG)

Before installing it on the other machines, I can verify that it has been installed and is working correctly by checking in the Windows Services:

![Zabbix 32](images/zbx32.PNG)

Then, I proceed to add the host to which I have just installed the agent with its corresponding data (Name: SV02, IP: 192.168.1.6, etc.).

![Zabbix 33](images/zbx33.PNG)

![Zabbix 34](images/zbx34.PNG)

From now on, I can collect information from the SV02 server, and visualize it from MON01 (Linux Debian 12.5):

![Zabbix 35](images/zbx35.PNG)

### Grafana Installation

To begin the installation of Grafana, I go to their official website --> [https://grafana.com/](https://grafana.com/).

In this case, I will choose to install from the apt package manager, for the sake of simplicity and complete integration with the operating system. Besides, it has the advantage of automatic updates when I run the "apt update" command.

Another option is to install by downloading and installing a ".deb" file, which has advantages such as access to the latest version and full control of the installation.

![Grafana 1](images/grf1.PNG)

To start with the installation, I perform the following steps:

![Grafana 2](images/grf2.PNG)

![Grafana 3](images/grf3.PNG)

The packages installed in the above image are required as they provide the following:

  - **apt-transport-https**: Enables support for HTTPS connections in apt.
  - **software-properties-common**: Provides additional tools to manage software repositories.
  - **wget**: Used to download files from the command line.

![Grafana 4](images/grf4.PNG)

![Grafana 5](images/grf5.PNG)

These commands are used to add the Grafana GPG (Gnu Privacy Guard) key to the system, allowing to verify the authenticity of the packages downloaded from the Grafana repository. 

  - **sudo mkdir -p /etc/apt/keyrings/**: mkdir -p creates the specified directory (/etc/apt/keyrings/). The -p option ensures that if the directory already exists, an error is not generated.
  
  - **wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null**: Downloads the Grafana GPG key and saves it in /etc/apt/keyrings/grafana.gpg

The last command (**ls -l /etc/apt/keyrings/**) was executed only to verify that the key was downloaded correctly.

![Grafana 6](images/grf6.PNG)

![Grafana 7](images/grf7.PNG)

The command
  - **echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list** 

is used to add a Grafana repository line to the apt source configuration file on Debian-based systems (such as Ubuntu).

![Grafana 8](images/grf8.PNG)

![Grafana 9](images/grf9.PNG)

Now, I ran the command   
  
  - **sudo apt-get update**  

to update the list of available packages.

When you run "sudo apt-get update", the system contacts each of the repositories listed in /etc/apt/sources.list and in the files inside the /etc/apt/sources.list.d/ directory (such as grafana.list that we configured earlier).

![Grafana 10](images/grf10.PNG)

![Grafana 11](images/grf11.PNG)

The 

  - **sudo apt-get install grafana** 
  
command is used to install Grafana on a Debian-based system, such as Ubuntu.

![Grafana 12](images/grf12.PNG)

Finally, I run the command

  - **apt list --installed | grep grafana**

to verify that grafana was installed correctly.

![Grafana 13](images/grf13.PNG)

Finally I run the commands  

  - **systemctl start grafana-server**
  - **systemctl enable grafana-server**

The systemctl start grafana-server command is used to manually start the Grafana service on the system. 

The systemctl enable grafana-server command is used to configure Grafana to start automatically at system startup.

### Zabbix and Grafana Integration

To access the Grafana web portal, I go to the address (localhost:3000) or simply use my hostname (deb-mon01:3000) or my ip address (192.168.0.7:3000):

![Grafana 14](images/grf14.PNG)

By default, I can login with the following credentials:

    - user: admin
    - password: admin

![Grafana 15](images/grf15.PNG)

Now, I must integrate Zabbix with Grafana. 

- **Zabbix** is a network and system monitoring platform that allows you to monitor and track the performance of IT infrastructures, networks, services and applications.

- **Grafana** is an open source data visualization and analysis platform designed to work with time series and metrics data.

Grafana can be integrated with Zabbix as a data source, allowing users to import data from Zabbix and create custom visualization dashboards that combine data from multiple sources.

To integrate Zabbix with Grafana, I perform the following steps:

1. First, I verify that all the devices are recognized in Zabbix. 
  To verify this, I go to deb-mon01/zabbix/, then inside the web interface I go to Data Collection and then Hosts.
  In this case, I have 2 virtual machines turned off (JMFSOFT-PC03 and JMFSOFT-PC04) for a lack of resources, but in a real environment they would all be on. <br/><br/>
  ![Grafana 16](images/grf16.PNG)

2. Then, once this is confirmed, I must log in to the Grafana web interface.
  To do this, I go to deb-mon01:3000: <br/><br/>
  ![Grafana 17](images/grf17.PNG)

3. Once this is done, in the left panel, I go to Administration --> Plugins: <br/><br/>
   ![Grafana 18](images/grf18.PNG) <br/><br/>
   Inside Plugins, I look for the "Zabbix" plugin and install it: <br/><br/>
   ![Grafana 19](images/grf19.PNG) <br/><br/>

4. Now, once the plugin is installed and enabled, I must restart the grafana-server service, with the following command:
    - **sudo systemctl restart grafana-server** <br/><br/> 
    ![Grafana20](images/grf20.PNG) <br/><br/> 

5. Now, I must link the Grafana service to Zabbix. <br/>
  Within the web interface, from the Connection console, I go to Data Sources and then select "Add Data Source".<br/> <br/> 
  Once there, I choose "Zabbix" and complete the following fields: <br/>

     - **Name**: Zabbix Server
     - **URL**: http://deb-mon01/zabbix/api_jsonrpc.php 
     - **Username**: Admin
     - **Password**: zabbix

    Although it is recommended to create a dedicated account especially for this instead of using the "Admin" account, for the sake of simplicity, I have decided to use the Admin account.

    ![Grafana21](images/grf21.PNG) <br/><br/> 
    ![Grafana22](images/grf22.PNG) <br/>

    Once this is saved, a green banner should be displayed indicating that you have successfully connected to the data source. <br/> If not, a red banner is displayed indicating an error.
    
    ![Grafana23](images/grf23.PNG) <br/>

6. Once the connection with the data source is made, I can proceed with the creation of Dashboards.
   
   To do this, fill in the "**Host**" field with the host name of the device to be monitored.
    For example: DC01.
    Then, I fill in the "**Item**" field with the item I want to monitor. For example, if I want to see the CPU utilization, I fill in "Windows: CPU Utilization":

    ![Grafana24](images/grf24.PNG) <br/>

    In this case, the usefulness of the graph cannot be fully appreciated since the server was inactive and therefore the CPU utilization is practically null, but in a real environment its use could be analyzed over time.

    I decided to use a "**Time Series**" type graph because it is the most appropriate for the following reason: 
    CPU utilization varies continuously as processes run on the server. A time series graph captures these changes in real time and allows you to see how the CPU load fluctuates over the course of the day, week, month or any other desired time period. In addition, patterns and trends can be analyzed.

    For demonstration purposes, I also decided to add the "**Disk Write Rate**" to the dashboard.
  
7. Finally, I created another panel and added it to the dashboard.
  This completes the installation of Zabbix, the installation of Grafana, and the subsequent integration of Zabbix with Grafana for visualization.

    ![Grafana25](images/grf25.PNG) <br/>

-------------------------------------------------------------------------------------------------

## Task Automation with PowerShell

PowerShell is a valuable tool for automating tasks in a Help Desk role because it saves time and reduces manual work. Instead of performing repetitive tasks like resetting passwords or creating user accounts one by one, you can automate these processes with scripts, completing them in seconds. This not only speeds up your workflow but also allows you to handle more requests in less time.

Automating with PowerShell also ensures consistency and reduces human error. When tasks are done manually, there’s always a risk of mistakes, but a well-written script guarantees the same accurate result every time. This makes PowerShell an essential tool for ensuring that systems are properly managed and maintained.

Additionally, PowerShell offers flexibility, allowing you to manage different aspects of IT, from Active Directory to software deployment, making it a versatile solution for a wide range of tasks.

![Powershell-logo](images/powershell-logo.jpg)

### Bulk User creation through Powershell

Using PowerShell to create users in Active Directory offers key advantages like automation, allowing repetitive tasks to be performed quickly and consistently, ensuring uniformity across the organization. It also provides scalability, making it efficient to manage large numbers of users, and flexibility to customize attributes during creation, all of which help streamline user management in enterprise environments.

To perform a mass creation of users using Powershell, I must perform the following steps:

1. First, I am going to create a file with .csv (Comma-Separated Value) extension, which will contain the users that I am going to add to Active Directory using Powershell.

    This file, as the name of the extension indicates, will separate each Active Directory attribute by commas (,).

    ![Pwsh1](images/pwsh1.PNG)
   
    
2. Now, I must prepare the script that imports that .csv file, and run the command that creates the users in Active Directory based on that file.

    First, I import the CSV file. 

    Then, I create some variables that will be useful to pass as parameters to the command that will create the users. 

    In this case, for testing purposes, all users will go to the same OU, and the domain will be one created on a Windows Server 2016 server, named 'adatum.local'.

    ![Pwsh2](images/pwsh2.PNG)


3. Once the .csv file is loaded in the $users variable, I proceed to go through each one of the users and for each one of them (forEach), I save their data in variables and pass them to the New-ADUser object, which will create the users.

    ![Pwsh3](images/pwsh3.PNG)

    The console result is as follows:

    ![Pwsh4](images/pwsh4.PNG)

    And the result of executing the script in Active Directory is this (the user 'Juanma' was already created): 

    ![Pwsh5](images/pwsh5.PNG)


### Bulk User Disabling through Powershell

To disable users in Powershell, the command 'Disable-ADAccount' is used.

In this case, I will disable those users that belong to the 'IT' department.

The script is simple, since the command only receives the user's identity.

![Pwsh6](images/pwsh6.PNG)

-------------------------------------------------------------------------------------------------

## Project Conclusion

### **Project Overview**:

This project demonstrates my capabilities in setting up and managing a comprehensive IT environment, essential for a role as an IT Support Specialist or Help Desk Jr/Trainee. The core of the project is the creation of a homelab that simulates real-world IT infrastructure, showcasing a range of skills from system administration to network monitoring, user support, and cloud-based services.

### **Tools and Technologies Used**:

1. **Virtualization**:
  
    - **Hyper-V**: Used to create and manage multiple virtual machines (VMs), allowing for a dynamic and scalable test environment.

      ![Hyper-V](images/hyperv-logo.jpg)

2. **Operating Systems**:

    - **Windows Server 2022**: Deployed as domain controllers and application servers.

      ![Windows-SV](images/windows-sv-logo.jpg)

    - **Windows 10**: Set up as client machines.  
    
      ![Windows-10](images/windows10-logo.jpg)

    - **Linux (Debian)**: Configured for monitoring and visualization.

      ![Debian](images/debian-logo.png)

3. **Directory Services**:
 
    - **Active Directory (AD)**: Implemented to manage users, groups, and devices across the network.

      ![Active Directory](images/ad-logo.jpeg)

    - **Azure Active Directory**: Set up for cloud-based identity and access management.

      ![Azure Active Directory](images/aad-logo.png)

4. **Network and System Management**:

    - **PDQ Deploy**: Utilized for automated software deployment and management.

      ![PDQ-Deploy](images/pdq-logo.png)

    - **ADUC (Active Directory Users and Computers)** and **ADAC (Active Directory Administrative Center)**: Employed for detailed management of the AD environment.  

5. **Cloud Services**:
   
    - **Microsoft 365**: Implemented and managed user accounts, licenses, and shared mailboxes. Configured services like Exchange, SharePoint, and OneDrive, including recovery of deleted files and mailbox management.

      ![Microsoft-365](images/m365-logo.png)

6. **Monitoring and Visualization**:
   
    - **Zabbix**: Installed on a Linux VM for network and system monitoring.
  
      ![Zabbix](images/zabbix-logo.png)

    - **Grafana**: Used to create visual dashboards for monitoring data from Zabbix.

      ![Grafana](images/grafana-logo.jpg)

7. **Ticketing System**:
   
    - **Jira Service Management (JSM)**: Adopted to simulate real-world IT ticketing and service management.

      ![Jira Service Management](images/jsm-logo.jpg)

8. **Remote Access and Support**:
  
    - **TeamViewer**: Integrated for remote access and support capabilities, facilitating troubleshooting and user assistance.

      ![TeamViewer](images/teamviewer-logo2.jpg)

9.  **VPN Configuration**:
    
    - **Remote Access Role (Windows Server)**: Set up a VPN to allow secure remote access to the network, including the configuration of roles and client connectivity.
   
      ![VPN](images/vpn-logo.jpg)

10. **Task Automation**:

    - **Powershell**: PowerShell scripts were used to automate key tasks such as bulk user creation and bulk user disabling in Active Directory, improving efficiency and consistency across the project.

      ![Powershell](images/powershell-logo.jpg)

### Key Project Components:

1. **Homelab Setup**:

    - Created a fully functional homelab consisting of two Windows Server 2022 VMs, four Windows 10 client VMs, and a Linux VM.    
    - Configured the VMs to replicate a corporate IT environment, including domain joining and policy application.

2. **Active Directory Deployment and Management**:

    - Set up a primary domain controller with Windows Server 2022.
    - Implemented organizational units (OUs) to mirror departmental structures.
    - Created user accounts and assigned them to appropriate OUs, simulating real-world departmental hierarchies.
    - Integrated Group Policy Objects (GPOs) to enforce security policies, manage desktop configurations, and ensure uniformity across the domain.
  
3. **Cloud Identity and Security**:

    - Established and configured Azure Active Directory (Azure AD) for cloud-based identity management.
    - Created and managed cloud-based user accounts and groups.
    - Enabled Multi-Factor Authentication (MFA) for added security on Azure AD.

4. **System and Software Management**:

    - Utilized PDQ Deploy for centralized software deployment across all Windows clients, demonstrating proficiency in remote software management.
    - Configured and managed Windows Server roles and features, including DNS, DHCP, and Remote Access.

5. **Microsoft 365 Administration:**:
    
    - Managed Microsoft 365 services, including user accounts, license assignments, and group configurations.
    - Administered Exchange Online for mailbox management, and implemented SharePoint and OneDrive for file storage and collaboration.
    - Executed recovery operations for deleted files and mailboxes.

6. **Network Monitoring and Visualization**:

    - Installed and configured Zabbix for comprehensive monitoring of all VMs.
    - Set up Grafana dashboards to visualize monitoring data, providing insights into system performance and health.

7. **IT Service Management**:

    - Implemented Jira Service Management (JSM) as a ticketing system to simulate handling IT support requests.
    - Managed and resolved simulated tickets, reflecting typical IT support tasks like password resets, software installations, and system troubleshooting.

8. **Remote Access and Support**:

    - Integrated tools like TeamViewer and AnyDesk for remote access and support capabilities.
    - Demonstrated proficiency in using these tools to resolve user issues remotely, enhancing support efficiency.

9. **VPN Configuration**:
    
    - Configured a VPN to provide secure remote access to network resources, including role-based access controls and client setup.

10. **Task Automation with Powershell**:

    - Automated key tasks such as bulk user creation and bulk user disabling in Active Directory, reducing manual intervention and improving efficiency.
  
-------------------------------------------------------------------------------------------------
