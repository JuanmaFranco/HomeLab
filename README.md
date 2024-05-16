# Active Directory Home Lab - Personal Project

![Home Lab Image](images/homelab.jpg)

Welcome to my personal project of creating a homelab utilizing Active Directory, both on-premises and in the cloud with Azure Active Directory. This project showcases the step-by-step creation of the homelab, including skills acquired such as user and group creation, password resets, and various other administrative tasks.

-----------------------------------------------------------------------------------------------------

# Table of Contents

1. [Homelab Creation](#homelab-creation)
2. [User and Group Creation](#user-and-group-creation)
3. [Password Reset](#password-reset)

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

1. **Set up Hyper-V** on your host machine.
2. **Create Virtual Machines**:
    - Two server VMs: One for the Domain Controller and another for additional services.
    - Four client VMs: These will simulate the end-user workstations.
3. **Install the necessary operating systems** on all VMs (e.g., Windows Server for servers and Windows 10/11 for client machines).
4. **Configure the Domain Controller**:
    - Install Active Directory Domain Services (AD DS) on the first server.
    - Promote it to a Domain Controller and create a new domain.
5. **Join all client machines** to the domain.
6. **Create Organizational Units (OUs)** in Active Directory for each department.

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

15. **Lara Vega <3**
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


## Password Reset
Content for password reset...
