# Windows Server Enterprise AD & GPO Environment 🏢

![Windows Server](https://img.shields.io/badge/Windows_Server-OS-0078D6?style=flat-square&logo=windows)
![Active Directory](https://img.shields.io/badge/Active_Directory-IAM-0078D6?style=flat-square&logo=microsoft)
![GPO](https://img.shields.io/badge/Group_Policy-Management-5C2D91?style=flat-square)
![Windows 11](https://img.shields.io/badge/Windows_11-Endpoint-0078D4?style=flat-square&logo=windows11)
![Status: WIP](https://img.shields.io/badge/Status-Work_In_Progress-yellow?style=flat-square)
<!--![Status](https://img.shields.io/badge/Status-V1.0_Done-green?style=flat-square)-->

This project addresses the challenge of decentralized user and device management in a corporate environment. By deploying a Windows Server 2025 Domain Controller, it establishes a centralized Active Directory infrastructure with Role-Based Access Control. It provides automated resource provisioning and enforces strict security polices across client workstations, simulating a real-world enterprise IT deployment.

## 🛠️ Technologies
* Windows Server 2025 (Evaluation)
* Windows 11 Pro (Evaluation)
* Active Directory Domain Services
* Domain Name System
* Group Policy Management
* VMware Workstation

## ✨ Features
* Centralized identity and access management for corporate users.
* Hierarchical Organizational Unit structure reflecting actual company departments (HT, IT, Accounting).
* Enforced security baselines (e.g., blocking Control Panel access for standard users).
* Automated network drive mapping based on departmental group membership.
* Seamless joining of endpoint devices to the corporate domain.

## ⚙️ The Process
* **Architecture Setup:** Configured a static IP for the Windows Server and established a private virtual network to isolate the lab environment.
* **AD DS & DNS Installation:** Installed the required roles and promoted the server to a Domain Controller, creating the `caprincho-corp.local` forest.
* **OU & User Provisioning:** Designed the initial OU hierarchy and created standard user accounts, enforcing the "User must change password at next login" security standard.
* **GPO Deployment:** Created and linked specific Group Policy Objects to restrict system settings globally and map SMB shares exclusively for the HR department.
* **Endpoint Integration:** Configured the Windows 11 VM's DNS to poing to the Domain Controller and successfully joined the workstation to the domain to verify policy application.

## 📊 Proof of Concept / Testing
* [ADUC Structure](Documentation/aduc_structure.png)
  </br>*Active Directory Users and Computers console displaying the mapped organizational structure.*
* [GPO Management](Documentation/gpo_management.png)
  </br>*Group Policy Management console showing linked security and resource policies.*
* [Blocked Control Panel](Documentatnion/blocked_control_panel.png)
  </br>*Windows 11 client side: Standard user receiving and "Access Denied" prompt when attempting to open the Control Panel.*
* [Mapped Drive](Documentatnion/mapped_drive.png)
  </br>*Windows 11 client side: HR user successfully receiving the mapped 'H:' drive upon login.*

## 💡 What I Learned
* Deepened understanding of AD DS architecture and the critical role DNS plays in domain environments.
* Learned how to effectievely structure OUs to make GPO application logical, scalable and manageable.
* Mastered the mechanics of joining a client OS to domain and managing local vs. domain credentials.
* Improved troubleshooting skills regarding network connectivity and DNS resolution between virtual machines.

## 🚀 What can be improved
* Automate the entire user provisioning process using PowerShell scripts reading from a CSV file (coming up in Version 2.0).
* Implement a secondary Domain Controller for High Availability and replication testing.
* Add advanced GPOs, such as deploying software automatically or mapping specific network printers.

## How to run the Project
1. Download Windows Server 2025 and Windows 11 ISOs form the Microsoft Evaluation Center.
2. Create two virtual machines in your Hypervisor and place them on the same internal virtual network.
3. Assign a static IP (e.g., `192.168.10.10`) to the Windows Server and install the AD DS and DNS roles via Server Manager.
4. Promote the server to a Domain Controller (create a new forest, e.g., `corp.local`).
5. On the Windows 11 VM, open Network Adapter Settings and set the Preferred DNS server to the IP of your Domain Controller (`192.168.10.10.`).
6. Join the Windows 11 VM to the domain via Settings -> Accounts -> Access work or school, click Connect, and select "Join this device to a local Active Directory domain". Enter the domain name, followed by credentials with domain-join permissions, then restart.
7. Use `dsa.msc` and `gpmc.msc` on the server to recreate the OU structure and test GPOs by logging into the Windows 11 VM with a domain account.
