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
* **Architecutre Setup:** Configured a static IP for the Windows Server and established a private virtual network to isolate the lab environment.
* **AD DS & DNS Installation:** Installed the required roles and promoted the server to a Domain Controller, creating the `caprincho-corp.local` forest.
* **OU & User Provisioning:** Designed the initial OU hierarchy and created standard user accounts, enforcing the "User must change password at next login" security standard.
* **GPO Deployment:** Created and linked specific Group Policy Objects to restrict system settings globally and map SMB shares exclusively for the HR department.
* **Endpoint Integration:** Configured the Windows 11 VM's DNS to poing to the Domain Controller and successfully joined the workstation to the domain to verify policy application.

## 📊 Proof of Concept / Testing


## 💡 What I Learned


## 🚀 What can be improved


## How to run the Project
