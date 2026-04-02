# Windows Server Enterprise AD & GPO Environment 🏢

![Windows Server](https://img.shields.io/badge/Windows_Server-OS-0078D6?style=flat-square&logo=windows)
![Active Directory](https://img.shields.io/badge/Active_Directory-IAM-0078D6?style=flat-square&logo=microsoft)
![GPO](https://img.shields.io/badge/Group_Policy-Management-5C2D91?style=flat-square)
![DHCP](https://img.shields.io/badge/DHCP-Network_Services-0078D6?style=flat-square)
![DNS](https://img.shields.io/badge/DNS-Resolution-0078D6?style=flat-square)
![IIS](https://img.shields.io/badge/IIS-Web_Server-0078D6?style=flat-square)
![Windows 11](https://img.shields.io/badge/Windows_11-Endpoint-0078D4?style=flat-square&logo=windows11)
![Status: V1.4](https://img.shields.io/badge/Status-V1.4_File_Shares_&_Delegation-brightgreen?style=flat-square)

This project addresses the challenge of decentralized user and device management in a corporate environment. By deploying a Windows Server 2025 Domain Controller, it establishes a centralized Active Directory infrastructure with Role-Based Access Control. It provides automated resource provisioning, zero-touch network configuration via DHCP, optimized DNS resolution, an internal Web Portal (Intranet), granular file sharing with NTFS security and enforces strict security policies across client workstations, simulating a real-world enterprise IT deployment.

## 🛠️ Technologies
* Windows Server 2025 (Evaluation)
* Windows 11 Pro (Evaluation)
* Active Directory Domain Services
* Domain Name System - Forward & Reverse Lookup Zones
* Dynamic Host Configuration Protocol
* Internet Information Services
* File and Storage Services
* Remote Server Administration Tools (RSAT)
* Group Policy Management
* VMware Workstation

## ✨ Features
* **Zero-Touch Network Provisioning:** Automated distribution of IP addresses, Subnet Masks, Default Gateways and DNS configurations to endpoint devices via an authorized DHCP server.
* **Advanced DNS Resolution:** Optimized DNS infrastructure featuring Reverse Lookup Zones for PTR record resolution (critical for security logging/SIEM) and DNS Forwarders for seamless external internet access.
* **Intranet Web Portal:** Deployment of a centralized corporate web server (IIS) accessible via friendly DNS records (`intranet.caprincho-corp.local`) for internal communications.
* **Secure Resource Sharing:** Centralized network shares with strict Access Control Lists, combining Share Permissions and NTFS Permissions based on Security Groups.
* **Administrative Task Delegation:** Implemented "Helpdesk" roles by using the Delegation of Control Wizard, allowing standard users to perform specific administrative tasks (e.g., password resets) via RSAT without requiring Domain Admin rights.
* **Centralized Identity Management:** Secure access control for corporate users with forced password resets upon initial login.
* **Hierarchical OU structure:** Organizational Units reflecting actual company departments (HR, IT, Accounting).
* **Enforced security baselines:** Global policies blocking Control Panel and System Settings access for standard users.
* **Automated Resource Mapping:** Dynamic network drive mapping based on departmental group membership using Group Policy Preferences.

## ⚙️ The Process
* **Architecture Setup:** Configured a static IP (`172.16.10.10`) for the Windows Server and established a private virtual network to isolate the lab environment.
* **AD DS & DNS Installation:** Installed the required roles and promoted the server to a Domain Controller, creating the `caprincho-corp.local` forest.
* **OU & User Provisioning:** Designed the initial OU hierarchy and created standard user accounts, enforcing the "User must change password at next login" security standard.
* **GPO Deployment:** Created and linked specific Group Policy Objects to restrict system settings globally and map SMB shares exclusively for the HR department.
* **Endpoint Integration:** Configured the Windows 11 to acquire its network configuration dynamically via DHCP and successfully joined the workstation to the domain to verify policy application.
* **DHCP Implementation (V1.1):** Deployed and AD-authorized the DHCP server role. Configured an IPv4 scope (`172.16.10.1` - `172.16.10.254` with exclusions for static IPs `.1` - `.100`). Ensured clients automtically receive the Default Gateway (`172.16.10.2`) and resolve the Domain Controller as their primary DNS.
* **DNS Optimization (V1.2):** Created Reverse Lookup Zones for the `172.16.10.x` subnet to enable IP-to-hostname resolution. Verified DNS Forwarders to ensure reliable external internet connectivity for domain clients.
* **IIS Web Server Deployment (V1.3):** Installed the IIS Web Server role. Configured a dedicated 'A' record (`intranet`) in the DNS Forward Lookup Zone. Replaced the default IIS homepage with a custom `index.html` corporate portal deployed in the `C:\inetpub\wwwroot` directory, ensuring internal accessibility via a domain-friendly URL.
* **File Shares & Administrative Delegation (V1.4):**
    * Restructured OUs to reflect a logical building/department hierarchy.
    * Created dedicated Security Groups (e.g., "HR", "Finance").
    * Provisioned a shared network folder with customized Share and NTFS permissions, ensuring only specific Security Groups have "Modify" access.
    * Utilized the *Delegation of Control Wizard* on a specific OU to grant standard users the ability to reset user passwords using RSAT.

## 📊 Proof of Concept / Testing

![NTFS Permissions](Documentation/ntfs_permissions.png)
<br>*> Server side: Advanced Security Settings showing strict NTFS permissions applied to the departmental network share.*

![Delegation of Control](Documentation/delegation_control.png)
<br>*> Windows 11 client side: Standard user utilizing RSAT to sucessfully reset another employee's password via delegated permissions.*

![Intranet Portal](Documentation/iis_intranet.png)
<br>*> Windows 11 client side: Web browser successfully accessing the internal corporate portal via `intranet.caprincho-corp.local`.*

![DNS Resolution](Documentation/dns_nslookup.png)
<br>*> Windows 11 client side: Command prompt showing successful forward and reverse DNS resolution using the `nslookup` utility.*

![DHCP Lease](Documentation/dhcp_lease.png)
<br>*> Windows 11 client side: Command prompt showing `ipconfig /all` with successfully acquired DHCP lease, DNS and Gateway from the authorized Domain Controller.*

![ADUC Structure](Documentation/aduc_structure.png)
<br>*> Active Directory Users and Computers console displaying the mapped organizational structure.*

![GPO Management](Documentation/gpo_management.png)
<br>*> Group Policy Management console showing linked security and resource policies.*

![Blocked Control Panel](Documentation/blocked_control_panel.png)
<br>*> Windows 11 client side: Standard user receiving an "Access Denied" prompt when attempting to open the Control Panel.*

![Mapped Drive](Documentation/mapped_drive.png)
<br>*> Windows 11 client side: HR user successfully receiving the mapped 'H:' drive upon login.*

## 💡 What I Learned
* Integration of the IIS Web Server role with DNS to host and resolve internal corporate web applications.
* Deployment and AD-authorization of DHCP services to provide zero-touch network configuration for client endpoints.
* Deepened understanding of AD DS architecture and configured advanced DNS features (Reverse Lookup Zones, Forwarders).
* Learned how to effectively structure OUs to make GPO application logical, scalable, and manageable.
* Mechanics of joining a client OS to a domain and managing local vs. domain credentials.
* Implementation of the Principle of Least Privilege (PoLP) using the Delegation of Control Wizard to offload Helpdesk tasks to standard users securely.

## 🚀 What can be improved
* Automate the entire user provisioning process using PowerShell scripts reading from a CSV file (coming up in Version 2.0).
* Implement a secondary Domain Controller for High Availability and replication testing.
* Secure the internal IIS web server by deploying Active Directory Certificate Services (AD CS) to enable HTTPS/SSL.

## How to run the Project
1. Download Windows Server 2025 and Windows 11 ISOs from the Microsoft Evaluation Center.
2. Create two virtual machines in your Hypervisor and place them on the same internal virtual network.
3. Assign a static IP (e.g., `192.168.10.10`) to the Windows Server and install the AD DS, DNS, DHCP and Web Server (IIS) roles via Server Manager.
4. Promote the server to a Domain Controller (create a new forest, e.g., `corp.local`).
5. Configure DNS by adding a Reverse Lookup Zone for your subnet and setting up DNS Forwarders (e.g., `8.8.8.8`, `1.1.1.1`) and adding an `A` record for `intranet` pointing to the server's IP.
6. Authorize the DHCP server in Active Directory and configure a new IPv4 Scope (e.g., `192.168.10.0/24`) with proper DNS and Router options.
7. Set up IIS by navigating to `C:\inetpub\wwwroot`, removing default files, and adding a custom intranet page.
8. Create a shared folder on the `C:\` drive, adjust the Advanced Sharing options, and set strict NTFS permissions so only designated AD Security Groups have access.
9. On the Windows 11 VM, ensure the Network Adapter is set to obtain an IP address automatically (DHCP).
10. Join the Windows 11 VM to the domain via Settings -> Accounts -> Access work or school, click Connect, and select "Join this device to a local Active Directory domain". Enter the domain name, followed by credentials with domain-join permissions, then restart.
11. Install RSAT (Remote Server Administration Tools) on the Windows 11 client to test the delegated password reset functionality with a standard user account.
