# Active Directory Home Lab - Personal Project

![Home Lab Image](images/homelab.jpg)

Welcome to my personal project of creating a homelab utilizing Active Directory, both on-premises and in the cloud with Azure Active Directory. This project showcases the step-by-step creation of the homelab, including skills acquired such as user and group creation, password resets, and various other administrative tasks.

-----------------------------------------------------------------------------------------------------

# Table of Contents

1. [Homelab Creation](#homelab-creation)
2. [User and Group Creation](#user-and-group-creation)
3. [Password Reset](#password-reset)
4. [Software Deployment](#software-deployment)
5. [Monitoring and Visualization](#monitoring-and-visualization)

-----------------------------------------------------------------------------------------------------

## Homelab Creation

In this homelab setup, we will be using Hyper-V to create a virtualized environment. The primary components of this setup will include:

![Company Equipment](images/company-equipment.png)

1. **Two Servers**:
    - One server will act as the **Domain Controller (DC)**, responsible for managing the Active Directory (AD) and handling authentication and authorization within the domain.
    - The second server will be used for other purposes such as file storage, applications, or additional services required within the network.

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
|     2.1 | **Create the Domain Controller VM**: In Hyper-V Manager, click New > Virtual Machine, and follow the wizard to set up the VM with appropriate settings such as name, generation, memory, network, and virtual hard disk. Install an operating system later. <br/> In my case, I'm going to choose to change the default path to store the virtual machine, as it seems more organized to me. In this case, I'll opt to place it in C:\Hyper-V\VMs\\. <br/><br/> ![DC01-1](/images/dc01-1.PNG) <br/><br/> Afterwards, I select Generation 1, as Generation 2 can be prone to errors. <br/><br/> ![DC01-2](/images/dc01-2.PNG) <br/><br/> Now, I need to allocate the RAM to the virtual machine. In this case, I'll opt for the minimum recommended for Windows Server 2022 with Desktop Experience, which is 4GB (4096 MB). <br/> Additionally, I'll choose to leave the **'Use dynamic memory for this virtual machine'** checkbox checked. <br> When a virtual machine uses dynamic memory, it means that its memory allocation can adjust automatically based on the demand from the operating system and applications running within the virtual machine. Instead of assigning a fixed amount of memory, the virtual machine can request and release memory as needed, allowing for more efficient utilization of host resources. <br/><br/> ![DC01-3](/images/dc01-3.PNG) <br/><br/> Then, I select the network adapter to which the virtual machine will connect. In this case, it's the network adapter created in step 1.2 (WAN), which is associated with my Qualcomm Atheros QCA9377 wireless network card. <br/><br/> ![DC01-4](/images/dc01-4.PNG) <br/><br/> Next, I will create a virtual hard disk of 2TB (2048GB), which will be more than enough for any practice you may want to perform. <br/><br/> ![DC01-5](/images/dc01-5.PNG) <br/><br/> Finally, I select the option 'Install an operating system from a bootable CD/DVD-ROM' and locate the location of the .ISO file containing the Windows Server 2022 Operating System. <br/><br/> ![DC01-6](/images/dc01-6.PNG) <br/><br/> Once this is done, the virtual machine is successfully created. Now, all that's left is to connect to it, start it up, and follow the Windows Server installer. <br/><br/> ![DC01-7](/images/dc01-7.PNG) |
|     2.2 | **Create the Second Server VM**: Follow the same steps as above but name this VM Server2. <br/><br/> Next, I create the second virtual machine, which will also have Windows Server 2022 as its operating system. I will avoid detailing the installation as it is the same as the DC01 virtual machine, only the name is different. <br/> In this case, the virtual machine will be named 'SV02'. <br/><br/> ![SV02](/images/sv02.PNG) <br/><br/> Then, once all the fields are filled out, I have the 2 server machines created. <br/><br/> ![SV03](/images/sv02-2.PNG) |
|     2.3 | **Create the Client VMs**: Repeat the VM creation process four times for client machines, naming them JMFSOFT-PC01, JMFSOFT-PC02, JMFSOFT-PC03, and JMFSOFT-PC04. Assign at least 1024 MB of memory to each client VM. <br/><br/> Now, I repeat the process but instead of using the Windows Server 2022 .iso file as the boot disk, I choose the Windows 10 .iso file. <br/><br/> ![Client1](/images/client1.PNG) <br/><br/> ![Client1-2](/images/client1-2.PNG) <br/><br/> Then, once the 2 servers (Windows Server 2022) and the four client machines (Windows 10) are created, I have the following: <br/><br/> ![Client1-3](/images/client1-3.PNG)|
| **3. Install the necessary operating systems on all VMs** |   |
|     3.1 | **Prepare installation media**: Obtain ISO files for Windows Server (for the servers) and Windows 10/11 (for the client machines). <br/><br/> In this case, I chose to download both .iso files (Windows Server 2022 and Windows 10) beforehand, but you can also choose the option 'Install an operating system later,' which creates the virtual machine and waits for you to attach the desired .iso file later. <br/><br/> If you prefer to do it that way, you can download the .iso files from the official Microsoft website: <br/><br/> -  [Windows Server 2022](https://www.microsoft.com/es-ES/evalcenter/evaluate-windows-server-2022)<br/> -  [Windows 10](https://www.microsoft.com/es-es/software-download/windows10) <br/> |
|     3.2 | **Install Windows Server on DC1 and Server2**: Start the VM, connect to it, attach the Windows Server ISO, and follow the installation prompts to install Windows Server. Configure the server with appropriate settings (e.g., server name, IP address). <br/><br/> Once the virtual machine is created and the Windows Server 2022 disk is 'inserted,' I will start it and proceed with the installation. I will perform this process for both the DC01 (Domain Controller) server and the SV02 server. <br/><br/> ![DC-INS-1](/images/dc-ins-1.PNG) <br/><br/> After clicking 'Install,' I need to select the version I want. Among the options are the Standard and Datacenter versions, each with a Desktop Experience (GUI) version and a Server Core version. In this case, I will select the Standard version with Desktop Experience: <br/><br/> ![DC-INS-2](/images/dc-ins-2.PNG) <br/><br/> Next, I need to select the partition on which the operating system will be installed. In this case, it is the only partition I have: the 2TB partition created when I created the virtual machine, but I could also create another partition and install it there. <br/><br/> ![DC-INS-3](/images/dc-ins-3.PNG) <br/><br/> After selecting the partition, the installer will continue copying files and installing the operating system. The installation time varies depending on the resources, both those we have and those allocated to the virtual machine. <br/><br/> ![DC-INS-4](/images/dc-ins-4.PNG) <br/><br/> Now, once the above is completed, you will be prompted to create a password for the 'Administrator' user, who has full control over the operating system. This account is similar to the superuser in Linux. It is essential to remember this password. <br/><br/> ![DC-INS-5](/images/dc-ins-5.PNG) <br/><br/> Now, I can log in with the credentials provided recently. <br/><br/> ![DC-INS-6](/images/dc-ins-6.PNG) <br/><br/> Once logged in, the 'Server Manager' will automatically open (this can be disabled), which allows managing Roles and Features, configuring the server's Firewall, changing the name, changing the IP address, managing storage, etc. <br/><br/> In this case, within the 'Local Server' section, I will change its name and IP address. The IP address will change from being managed by DHCP to static, which is seen as a good practice (in servers, not in client machines), as it increases reliability among other reasons. <br/><br/> ![DC-INS-7](/images/dc-ins-7.PNG) <br/> <br/> The **name** will be: DC01. <br/><br/> ![DC-INS-8](/images/dc-ins-8.PNG) <br/><br/> The **static IP address** for DC01 will be: 192.168.1.5 <br/> The **subnet mask** will be: 255.255.255.0, which means that the first 3 octets of the IP address (192.168.1) will correspond to identifying the network portion, while the fourth and last octet will correspond to identifying the host portion. In this case, with the /24 subnet mask, we get 2^8 - 2 (254) available hosts for the devices and/or servers (2 raised to the available bits, minus 2 because there are 2 addresses that cannot be assigned: 192.168.1.0, which corresponds to the network address, and 192.168.1.255, which corresponds to the broadcast address). <br/> The **default gateway** will be 192.168.1.1, corresponding to the IP address of the default gateway that I have. <br/> The **primary DNS server** will be: 8.8.8.8, corresponding to Google's public DNS server. <br/> **The alternate DNS server** will be: 8.8.4.4, also corresponding to another public DNS server provided by Google. <br/><br/> ![DC-INS-9](/images/dc-ins-9.PNG) <br/><br/> To apply the changes (the server name change), it is necessary to restart the computer. <br/><br/> That's all for the DC01 server (temporarily, although it's still not a domain controller). Now, I will proceed to install the Windows Server operating system on the other server machine (SV02).  |
|     3.3 | **Install Windows 10/11 on client VMs**: Start each client VM, connect to it, attach the Windows 10/11 ISO, and follow the installation prompts to install Windows. Configure each client with appropriate settings (e.g., machine name, IP address). <br/><br/> Once the Windows 10 disk is 'inserted' into the client machine, I proceed to start it to initiate the installation process. <br/><br/> ![Win-01](/images/win-01.PNG) <br/><br/> Then, I press "Install now," and enter the product key (if available). In this case, I don't have one, so I select "I don't have a product key. <br/><br/> ![Win-02](/images/win-02.PNG) <br/><br/> Now, I select the version of Windows that I want to install. It's crucial to choose the Pro or Education version, as the Home version lacks the capability to join an Active Directory domain, thus cannot be centrally managed. In this case, I will opt for the Windows 10 Pro version. <br/><br/> ![Win-03](/images/win-03.PNG) <br/><br/> Now, I select again the partition on which I want to install the operating system. In this case, it's the only partition I have created. <br/><br/> ![Win-04](/images/win-04.PNG) <br/><br/> Now, I must wait for the installation process to finish. <br/><br/> ![Win-05](/images/win-05.PNG) <br/><br/> That's it. Now, once the operating system has started, I complete the final configurations (region, keyboard, network, local account, privacy, etc.), and once the desktop is displayed, I proceed to change the computer name to "JMFSOFT-PC01": <br/><br/> ![Win-06](/images/win-06.PNG) <br/><br/> Finally, I repeat the process for the rest of the computers, only varying the name (JMFSOFT-PC02, JMFSOFT-PC03, and JMFSOFT-PC04). |
| **4. Configure the Domain Controller** |   |
|     4.1 | **Install Active Directory Domain Services (AD DS):** Log in to DC1, open Server Manager, click Add roles and features, select Active Directory Domain Services, and complete the installation. <br/><br/> Now, to be able to use the Windows Directory Service (**Active Directory**), I need to install Active Directory Domain Services (AD DS) on the server designated as the domain controller. To do this, I go to the Server Manager and select the 'Add Roles and Features' option. Then, I select Active Directory Domain Services (AD DS) and follow the steps. <br/><br/> ![AD-01](/images/ad01.png) <br/><br/> In the next window, I select the 'Role-based or feature-based installation' option and also verify in the top left corner that the destination server matches the server where I want to install the role or feature (in this case, DC01, which is correct). <br/><br/> ![AD-02](/images/ad02.png) <br/><br/> Again, I select the desired server (DC01) and click Next. <br/><br/> ![AD-03](/images/ad03.png) <br/><br/> Next, I select the Active Directory Domain Services option. Clicking on this option will open an additional window that will identify all the roles and features that will be installed and are necessary for the proper functioning of this role. <br/><br/> ![AD-04](/images/ad04.png) <br/><br/> ![AD-05](/images/ad05.png) <br/><br/> After clicking the 'Add Features' option, the original Active Directory Domain Services checkbox will appear checked. <br/><br/> ![AD-06](/images/ad06.png) <br/><br/> Next, the features that can be installed on the server will appear. In this case, I don't need to add any, so I click Next. <br/><br/> ![AD-07](/images/ad07.png) <br/><br/> Finally, a summary of the installed roles and features will appear. I can also check the box indicating to the server that it should restart if necessary to complete the installation. This box should be checked carefully, as it is not appropriate to restart a server that is in operation in a business environment unless this restart has been scheduled in advance and does not affect the proper operation of any service. Once verified that everything is correct, I press 'Install' to begin the installation of AD DS. <br/><br/> ![AD-08](/images/ad08.png) <br/><br/> Once this is done, the installation will begin. I can close the window and let the installation run in the background or simply wait for it to finish. <br/><br/> ![AD-09](/images/ad09.png) <br/><br/> Finally, the installation is complete, but this doesn't mean that the server is already a Domain Controller, as only the Active Directory Domain Services (AD DS) were installed. To accomplish this, I must promote the server to a Domain Controller (DC), which will be done in the next step. <br/><br/> ![AD-10](/images/ad10.png) <br/><br/> |
|     4.2 | **Promote DC1 to a Domain Controller:** After installation, click on the notification flag in Server Manager, select Promote this server to a domain controller, choose Add a new forest, enter a root domain name (e.g., jmsoft.local), and complete the wizard. Restart the server as prompted. <br/><br/> Now, once Active Directory Domain Services are installed, I need to promote the server to a domain controller to create the forest, the domain, and then add the desired users. To do this, I need to go to the notification that appears in the Server Manager and click on 'Promote this server to a domain controller.' <br/><br/> ![AD-11](/images/ad11.png) <br/><br/> Once that option is selected, a window will open with three options: <br/><br/> **1 - Add a domain controller to an existing domain**: This option is useful when you want to add an additional server that provides authentication and authorization services within the same domain (a previously created domain). This increases redundancy and availability. It also helps distribute the load among multiple Domain Controllers, improving performance. <br/><br/> **2 - Add a new domain to an existing forest**: This involves creating an additional domain within an Active Directory forest. A forest is a collection of one or more domains that share a common schema and global catalog configuration. Domains within the same forest have implicit transitive trust relationships, allowing for authentication and access to resources across domains. <br/><br/>**3 -  Add a new forest**: This means creating a completely independent instance of Active Directory. A forest is the highest security and administrative boundary in AD. <br/><br/> In this case, I will select the last option (Add a new forest), and the root domain name will be '**jmfsoft.local**'. If there is a website called 'jmfsoft.com', it is good practice not to use that domain name as the root domain name to avoid DNS resolution conflicts. <br/><br/> ![AD-12](/images/ad12.png) <br/><br/> Once 'Next' is pressed, a window will open with multiple options: <br/><br/> ![AD-13](/images/ad13.png) <br/><br/> Forest functional level and domain functional level refer to the compatibility settings and features available in an Active Directory environment. These levels determine which Active Directory functions and features can be used, based on the versions of Windows Server running on the domain controllers in the environment. <br/><br/>1 - The "**Domain functional level**" specifies the features and functionalities available within a specific domain in Active Directory. <br/><br/>2 - The "**Forest functional level**" specifies the features and functionalities available across the entire Active Directory forest, which includes all domains within that forest.<br/><br/> By raising the functional levels of the domain or forest, all domain controllers must be running at least the minimum version of Windows Server required by the new functional level. Once the functional level of the forest or domain is raised, it cannot be reverted to a previous functional level without restoring from a backup. <br/><br/> Next, there is a section called 'Specify Domain Controller Capabilities,' which has three options: <br/><br/> **1 - Domain Name System (DNS) Server**: This option allows the Domain Controller to provide name resolution services that are essential for Active Directory functionality. DNS is critical for clients and servers to locate resources within the domain. AD depends on DNS to locate domain controllers, replication services, and other critical resources. SRV (Service Location) records in DNS enable clients to locate services such as domain controllers. <br/><br/>**2 - Global Catalog (GC)**: This option indicates that the domain controller contains a partial copy of all objects in the Active Directory forest. Domain controllers acting as GCs store key information and allow quick searches across the entire forest. During logon, the GC can provide information about universal group membership, which is crucial for user authentication and authorization. <br/><br/>**3 - Read-Only Domain Controller (RODC)**: This is a special type of domain controller designed for environments where the physical security of the server cannot be guaranteed, such as branch offices or remote locations. The RODC stores a read-only copy of the Active Directory database. It cannot make changes to AD, which helps protect the integrity of the directory if the server is physically compromised. By default, the RODC does not store user credentials (passwords) unless explicitly configured to do so. <br/><br/> Finally, we need to specify the **Directory Services Restore Mode (DSRM) password**, which is an important security measure in Active Directory domain controllers. It is used in directory recovery situations when the system is in a degraded state or when critical maintenance tasks need to be performed. The DSRM password allows logging in to a domain controller even if the operating system cannot start normally or if the Active Directory service is inaccessible. This provides emergency access to the domain controller in critical situations. <br/><br/> ![AD-14](/images/ad14.png) <br/><br/> Next, we must verify the NETBIOS name, which is a network identifier used in Microsoft operating systems to identify resources on a local network. In the context of Active Directory, the NetBIOS name is primarily used for backward compatibility and to enable communication with legacy systems that use this technology. <br/><br/> ![AD-15](/images/ad15.png) <br/><br/> Next, the paths will be displayed, indicating the location of the folder containing the database, the folder containing the log files, and the SYSVOL folder. <br/><br/>1 - **The database folder (C:\Windows\NTDS by default)** stores the Active Directory database, where all directory objects and attributes are stored, including users, groups, policies, and system configurations. The NTDS.DIT file is the primary Active Directory database file. It contains all directory data, such as users, groups, computer objects, etc. Active Directory domain controllers (DCs) access this database to provide directory services.<br/><br/>2 - **The log files folder (C:\Windows\NTDS by default)** contains the transaction log files that record all operations performed on the Active Directory database. Transaction log (LOG) files are sequential and are used to ensure the consistency and integrity of the Active Directory database. Each transaction performed in the database is recorded in these files before being committed and written to the database. In the event of system failure or data loss, the log files can be used to recover lost data or to restore the database to a consistent state.<br/><br/> 3 - **The SYSVOL folder (C:\Windows\SYSVOL by default)** stores shared data and group policies in an Active Directory environment. SYSVOL contains files and folders that are replicated among all domain controllers in the domain. This includes user login scripts, login policies, group policies, login script files, etc.<br/><br/> In summary, these folders are critical for the operation and integrity of an Active Directory environment. The database folder stores the AD database, the log files folder records transactions, and the SYSVOL folder stores shared data and group policies. <br/><br/> ![AD-16](/images/ad16.png) <br/><br/> Then, a summary of all the selected options and configurations is displayed. This allows reviewing the choices to confirm if I want to make a last-minute change before promoting the server as a domain controller. In my case, I reviewed all the options and they are correct, so I will proceed with the installation. <br/><br/> Additionally, I can copy the executed script which contains the commands executed by the operating system to perform all the above. This is useful if I want to run a script in Windows PowerShell and automate future installations. <br/><br/> ![AD-17](/images/ad17.png) <br/><br/> As a final step prior to installation (i.e., promoting the server to Domain Controller), prerequisites are checked to see if the system is suitable for promotion. In this case, although it threw some usual warnings, the system is suitable to be promoted to a domain controller. After this, I click on Install and wait for the installation to complete. When the promotion operation is finished, the system will automatically restart. <br/><br/> ![AD-18](/images/ad18.png) <br/><br/> After the server restarts, the login screen changes. Below the user and password, a message appears saying **'Sign in to JMFSOFT'**, indicating that, unless specified otherwise, I'll sign in to the domain with that name. This doesn't necessarily mean that the server is a domain controller, as the login process on client machines will be the same, but it does indicate that the server is now part of the JMFSOFT domain. <br/><br/> ![AD-19](/images/ad19.png) <br/><br/> After promoting the server to a domain controller, new sections will appear in the Server Manager that didn't exist before. For example, on the left side, you'll see AD DS (Active Directory Domain Services) and DNS (Domain Name System). Then, in the tools section, several new tools will appear, such as Active Directory Administrative Center (ADAC), DNS, Active Directory Domains and Trusts, Active Directory Sites and Services, Active Directory Users and Computers (ADUC), among others. <br/><br/> With this, I conclude step 4.2, which aims to promote the server to a domain controller. |
| **5. Join all client machines to the domain** |   |
|     5.1 | **Configure network settings:** Ensure that all client machines and the second server can resolve the domain by setting the DNS server to the IP address of DC1. <br/><br/> To join client computers (and also the secondary server) to the newly created domain (jmfsoft.local), the first thing I need to do is change the DNS server to the IP address of the domain controller. This is necessary for several reasons: <br/><br/>1 - **Name Resolution**: Active Directory relies on the DNS service to resolve domain names, such as the domain name you are joining. The DNS server configured on the client needs to correctly resolve these names to locate the domain and complete the join process. <br/><br/>2 - **Domain Controller Location**: The client needs to locate the Domain Controller to join the domain and obtain configuration information. The IP address of the Domain Controller is used to identify where the Active Directory infrastructure is located on the network. <br/><br/>3 - **Resource Records in DNS**: During the domain join process, the client needs to look up specific resource records (e.g., SRV records) in DNS to locate Active Directory services, such as authentication and replication services. These records are associated with the domain name and are resolved via the DNS server configured on the client.<br/><br/>4 - **Communication with the Domain Controller**: After joining the domain, the client must be able to communicate with the Domain Controller to authenticate users, obtain group policies, and perform other Active Directory operations. Configuring the DNS server is essential for the client to correctly communicate with the Domain Controller. <br/><br/> If I do not point the DNS server to the IP address of the server, I get the following error: <br/><br/> ![Dom-1](/images/dom-1.png) <br/><br/> So, to solve this issue and allow the client machines (and the additional server) to join the 'jmfsoft.local' domain, I go to the network settings and set the DNS server to the IP of the domain controller, which in this case is 192.168.1.5. Although in some parts of the project the network is shown as both 192.168.0.0/24 and 192.168.1.0/24, this is because I am doing it in two geographically distant locations with two different ISPs, so this number will vary at times, but the concept remains the same. <br/><br/> Therefore, to do this, I go to the wireless connection icon -> Network and Sharing Center -> Change adapter settings -> Select the network adapter and right-click on it -> Properties -> Internet Protocol Version 4 -> Properties and then set the DNS server address to match the domain controller's IP address. <br/><br/> ![Dom-2](/images/dom-2.png) <br/><br/> |
|     5.2 | **Join each client to the domain:** On each client machine, open System Properties (right-click on This PC > Properties > Advanced system settings), click on Change next to To rename this computer or change its domain or workgroup, select Domain, enter the domain name (e.g., jmsoft.local), provide the credentials of a domain user, and restart each client machine after joining the domain. <br/><br/> Once this is done, I should be able to join the domain correctly, as the client computer can now correctly resolve the domain name. Now, I will try to join again through 'Change the name of this PC (Advanced):' <br/><br/> ![Dom-3](/images/dom-3.png) <br/><br/> Once the network configurations are completed, I need to enter credentials. The user account I need to enter must have at least 'Add computer to the domain' permissions. This permission can be granted by a Domain Admin. <br/><br/> ![Dom-4](/images/dom-4.png) <br/><br/> Once the credentials are entered, a message will appear indicating that the computer has been successfully joined to the domain.  <br/><br/> ![Dom-5](/images/dom-5.png) <br/><br/> For this to take effect, simply restart the computer and log in with an account that belongs to the 'jmfsoft.local' domain: <br/><br/> ![Dom-6](/images/dom-6.png) <br/><br/> Now, at the next login, the same message that appeared on the Domain Controller will appear: 'Sign in to JMFSOFT: <br/><br/> ![Dom-7](/images/dom-7.png) <br/><br/> Simply enter the credentials of a user who belongs to the domain, and step 5.2 is complete. Next, I will perform the same process (joining devices to the domain) with the rest of the client computers and the additional server. |
| **6. Create Organizational Units (OUs) in Active Directory for each department** |   |
|     6.1 | **Open Active Directory Users and Computers (ADUC) on DC1**: Go to Server Manager > Tools > Active Directory Users and Computers (ADUC) or Active Directory Administrative Center (ADAC). <br/><br/> **1 - ADAC (Active Directory Administrative Center):** <br/><br/> ![ADAC1](/images/adac1.png) <br/><br/> **Modern User Interface**: ADAC has a more modern and intuitive user interface, based on PowerShell and Windows Presentation Foundation (WPF) technologies. It was introduced with Windows Server 2008 R2. <br/><br/> **Advanced Features**: It offers advanced features such as the Active Directory Recycle Bin, which allows the recovery of deleted objects without having to restore the entire system. It enables more easily configurable Role-Based Access Control (RBAC). It incorporates PowerShell Search and command history, which facilitates task automation. <br/><br/> **Improved Security**: It supports smart card authentication and other enhanced authentication methods. <br/><br/> **Simplified Management**: It provides a consolidated and simplified view to manage Active Directory objects, such as users, groups, organizational units, and others. <br/><br/> **2 - ADUC (Active Directory Users and Computers):** <br/><br/> ![ADUC-1](/images/aduc1.png) <br/><br/> **Classic User Interface**: ADUC has a more traditional user interface, based on the Microsoft Management Console (MMC) snap-in. It is an older tool compared to ADAC and has been used since Windows 2000. <br/><br/> **Basic Features**: It allows basic management of users, groups, computers, and organizational units in Active Directory. It does not have direct access to some of the more advanced features available in ADAC, such as the AD Recycle Bin. <br/><br/> **Common Use in Classic Scenarios**: Although it is more basic, it is still widely used for daily administration tasks due to its familiarity and simplicity. <br/><br/> **Compatibility**: It is compatible with earlier versions of Windows Server and remains a reliable tool for direct management of objects in Active Directory. <br/><br/> |
|     6.2 | **Create OUs**: Right-click on the domain (e.g., jmsoft.local) and select New > Organizational Unit. Create OUs for each department (e.g., IT, Finance, Sales, HR, Marketing, Development, Customer Service, Design, Administration). <br/><br/> To create organizational units (OUs), I have two options: do it from Active Directory Users and Computers (ADUC) or from Active Directory Administrative Center (ADAC): <br/><br/> **1 - Create from Active Directory Users and Computers (ADUC)**: To do this, I go to Server Manager, Tools, and select Active Directory Users and Computers. Then, once inside ADUC, I expand the domain (jmfsoft.local) and select where I want to create the organizational unit. <br/><br/> ![OU-1](/images/ou1.png) <br/><br/> It is important to note that organizational units (OUs) cannot be created within containers, nor can group policies (GP) be applied to them. In this case, I will create an organizational unit called 'Argentina' within the domain and then create within it all the OUs related to the departments of the company at the Argentina site (assuming it is a company with many locations): <br/><br/> ![OU-2](/images/ou2.png) <br/><br/> **2 - Create from Active Directory Administrative Center (ADAC)**: To do this, I go to Server Manager, Tools, and select Active Directory Administrative Center. Then, once inside, I select the location, right-click, New, and select 'Organizational Unit'. <br/><br/> ![ADAC-2](/images/adac2.png) <br/><br/> Then, I complete the creation of the company's organizational units (IT, HR, Finance, etc.). <br/><br/> ![ADAC-3](/images/adac3.PNG) <br/><br/> |
|     6.3 | **Move user accounts to OUs**: After creating user accounts, move each account to the appropriate OU by right-clicking on the user, selecting Move, and choosing the appropriate OU. <br/><br/> Although user creation is part of the next section, I will create a test user and move it to a previously created organizational unit (in this case, IT) just to complete point 6.3 and finish with section 1 (Homelab Creation). <br/><br/> In this case, I will create the user named 'Prueba' in Customer Service and then move it to the IT Organizational Unit. <br/><br/> ![NewUser](/images/newuser.PNG) <br/><br/> Then, to move the user, I simply right-click on it and select 'Move...'. Now, I just need to choose which organizational unit (OU) to move it to. In this case, I will move it to IT: <br/><br/> ![NewUser2](/images/newuser2.PNG) <br/><br/> Once done, the user has been successfully moved from 'Customer Service' to 'IT'. <br/><br/> ![NewUser3](/images/newuser3.PNG) |

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

![Jira](/images/jsm1.PNG)

In this case, I have already created a JSM (Jira Service Management) project previously. Now, I will create the ticket that simulates the experience of receiving it, 'working on it', and resolving it.

![Ticket](/images/ticket.PNG) 
<br/> ..... 
<br/> ..... <br/>
![Ticket00](/images/ticket00.PNG)

After simulating the ticket sent by Carlos Pérez (a user created by myself), I assign it to myself (indicating that I will handle the ticket) and start creating the users.

Finally, once the ticket is resolved (with all the users created), I get the following: 

![Users](/images/users.PNG)

Therefore, once I have created all the requested users, I can mark the ticket as resolved and additionally leave a message for the user who initiated it, indicating that all the users have been successfully created. 

![Ticket2](/images/ticket2.PNG)

### User creation through Azure Active Directory (currently Microsoft Entra ID)

In addition to managing Active Directory on-premises, you can use the cloud version, which is Azure Active Directory, currently known as Microsoft Entra ID.

![Azure1](/images/azure1.PNG)

To do this, I must first have my domain created (in this case, I created the domain jmfsoft.onmicrosoft.com).

To create users, I go to the Users section and select New User -> Create New User:

![Azure2](/images/azure2.PNG)
![Azure3](/images/azure3.PNG)

Then, I fill in the desired fields.

![Azure4](/images/azure4.PNG)

![Azure5](/images/azure5.PNG)

Finally, I check that all the fields have been filled in correctly:

![Azure6](/images/azure6.PNG)

If everything is correct, I press 'Create' to finish creating the user:

![Azure7](/images/azure7.PNG)

### Bulk user creation with Azure Active Directory using a .csv file

To create multiple users at once, Azure Active Directory has a section that allows you to create multiple users based on an uploaded .csv file.

To do this, I go to Bulk operations -> Bulk Creation and download the corresponding .csv.

![Azure8](/images/azure8.png)

![Azure9](/images/azure9.PNG)

Once the .csv is downloaded, I fill in each column with the corresponding data, where each row will correspond to a particular user.

Once all users are created, I get the following:

![Azure11](/images/azure11.PNG)

-------------------------------------------------------------------------------------------------

## Password Reset

Now, to continue with the homelab, I will practice another fundamental skill, which is resetting user passwords. Again, I will create a ticket from the same person to simulate a real environment.

Once the ticket is created and the request is submitted, I log in as 'IT Support' and view the ticket in question:

![Ticket-Reset](/images/ticket-reset.PNG)

To do this, I go to **Active Directory Users and Computers (ADUC)** or **Active Directory Administration Center (ADAC)**, and locate the user.

### Reset password on Active Directory Users and Computers (ADUC)

In this case, the search can be done through the organizational units (as marked with 1, which gave me the results of the design department) or through the search function, useful when we have many users (as marked with 2):

![Ticket-Reset2](/images/ticket-reset2.png)

Once I have located the user, I right click on it and 'Reset password':

![Ticket-Reset3](/images/ticket-reset3.png)

Now, I must enter a temporary password that will be given to the user to try to log in next time.

It is important to check the box that indicates that the user will have to change the password at the next login, this is a good practice and its use is recommended.

Finally, it gives us the possibility to unblock the user, although in this case the user did not actually block the account.

![Ticket-Reset4](/images/ticket-reset4.png)

Once this is done, I get a confirmation message:

![Ticket-Reset5](/images/ticket-reset5.png)

### Reset password on Active Directory Administrative Center (ADAC)

Resetting a password in the Active Directory Administrative Center is just as easy, in addition to the fact that it has a more user-friendly interface.

Once ADAC is open, we can locate a section where we can easily reset a user's password:

![Ticket-Reset6](/images/ticket-reset6.png)

If I do not opt for this method, I can simply move around the domain until I locate the corresponding organizational unit (OU) and then locate the user:

![Ticket-Reset7](/images/ticket-reset7.png)

Now, once I have located the user, I can choose to right click on it and select 'Reset password' (1) or simply use the right sidebar (2) which also has this option:

![Ticket-Reset8](/images/ticket-reset8.png)

Again, I fill in the field with the temporary password and select the option for the user to change it at the next login:

![Ticket-Reset9](/images/ticket-reset9.png)

Done! Once this is completed, the user must log in with the temporary password and then will be prompted to update it. 

### Reset password on Azure Active Directory

To reset a password in Azure Active Directory, I go to the users section.

![Ticket-Reset10](/images/ticket-reset10.png)

Then, I select the user to reset the password.

In this case, as the ticket indicated, the user is “Lara Vega”:

![Ticket-Reset11](/images/ticket-reset11.png)

Once in the user's profile, all you need to do is click on “Reset password” at the top and a sidebar will open:

![Ticket-Reset14](/images/ticket-reset14.png)

![Ticket-Reset12](/images/ticket-reset12.png)

Once the button is pressed, the user's password will be reset and a temporary password will be automatically generated, which must be given to the user so that he/she can change it later. 

In this case, the temporary password is “Xavo6347”.

![Ticket-Reset13](/images/ticket-reset13.png)

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

![PDQ](/images/pdq1.PNG)

Once registered, I am redirected to the following page where I get the links to download PDQ Deploy & PDQ Inventory, along with their licenses (which last for 14 days).

![PDQ2](/images/pdq2.PNG)

After installing (as a server, since SV02 will be in charge of deploying to client machines), the program interface will look like this:

![PDQ3](/images/pdq3.PNG)

Now, to begin the deployment, first, I need to have the installer of the software I want to deploy downloaded and saved in a location.

In this case, for simplicity, I'll choose to install 7zip.

The installer will be located in a folder on the desktop named 'Software to Deploy':

![PDQ4](/images/pdq4.PNG)

Now, I open PDQ Deploy and select 'New Package'. Then, I fill in the fields.

![PDQ5](/images/pdq5.PNG)
![PDQ6](/images/pdq6.PNG)

Afterwards, I go to 'Steps' and then select 'Install':

![PDQ7](/images/pdq7.PNG)

Then, I select the installer, add the parameters for silent installation (in this case, the appropriate one for 7zip is /S) to avoid disturbing any user while they are working, and press save.

![PDQ8](/images/pdq8.PNG)

Once this is done, the package will appear already created. Now, all that's left is to right-click on it and press 'Deploy Once' to begin the deployment.

![PDQ9](/images/pdq9.png)

Finally, I must select the users/computers by clicking on 'Choose Targets', and that's it, I can start the deployment:

![PDQ10](/images/pdq10.PNG)

In this case, just for testing purposes, I'll select client machines 1 and 2 (JMFSOFT-PC01 and JMFSOFT-PC02):

![PDQ11](/images/pdq11.PNG)

After initiating the deployment, I get the following results:

![PDQ12](/images/pdq12.PNG)

Now, I verify that 7zip was installed correctly on the client machines:

![PDQ13](/images/pdq13.PNG)

And, with this, I can successfully conclude the deployment.

-------------------------------------------------------------------------------------------------

## Monitoring and Visualization

In this section, I will install and integrate two programs, **Zabbix and Grafana**, to monitor client machines. This setup will enable real-time tracking and management of system performance and network activity. The primary reasons for including Zabbix, a robust monitoring tool, and Grafana, a powerful real-time data visualization program, in my homelab are:

- **Proactive Issue Detection**: Zabbix will help in identifying and addressing potential issues, such as hardware failures, software crashes, or network disruptions, before they impact the system's functionality. Grafana will provide a clear, visual representation of these issues, making them easier to understand and act upon.

- **Performance Optimization**: Continuous monitoring with Zabbix and real-time data analysis through Grafana will allow for fine-tuning system performance and ensuring efficient resource utilization.

- **Security Enhancement**: By using Zabbix to keep a close watch on system activities and Grafana to visualize security metrics, I can quickly detect any unauthorized access or unusual behavior, thereby strengthening the security of the environment.

- **Learning and Experimentation**: Implementing Zabbix and Grafana will provide hands-on experience in setting up, configuring, and utilizing monitoring and data visualization technologies, which are critical skills for IT management and cybersecurity.

![Zabbix Logo](/images/zabbix-logo.png)
![Grafana Logo](/images/grafana-logo.jpg)

I will proceed to install both tools on the secondary server (SV02) because it's generally advisable to keep monitoring and visualization tasks separate from the domain controller (DC01) to avoid overloading the primary domain controller and to ensure better performance and security. By using SV02 for these tasks, we can offload the workload from the DC01 and maintain a more organized and manageable server environment.

To start the installation, I go to the Zabbix website: [Zabbix](https://www.zabbix.com/), download it, and run the installer.