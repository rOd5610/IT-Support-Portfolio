# Active Directory Home Lab

## Scenario
I built a complete Active Directory domain for a fictional company called **TechBridge Solutions**. The domain `ROD.LOCAL` was set up on Windows Server 2025, with department OUs, security groups, Group Policy Objects, and a domain-joined Windows 10 client.

## Tools Used
- Windows Server 2025 (Evaluation)
- Windows 10 Enterprise
- Active Directory Domain Services
- VirtualBox
- Windows Administrative Tools (ADUC, GPMC)

---

## Domain Controller Setup

| Detail | Value |
| :--- | :--- |
| Server Name | DC01-TechBridge |
| Domain | ROD.LOCAL |
| Forest Functional Level | Windows Server 2025 |
| IP Address | 192.168.56.102 (Host-Only Adapter) |
| Virtualization Platform | Oracle VirtualBox |

---

## Network Configuration & Troubleshooting

### Initial Issue
When I first set up the domain controller, I used the **NAT** network adapter in VirtualBox. The client VM (Windows 10) was also on NAT. While both VMs could access the internet, they could not communicate with each other — the client could not ping the domain controller, and domain join attempts failed.

### Root Cause
- The NAT adapter in VirtualBox creates an isolated network that does not allow communication between VMs by default.
- For a domain controller and client to communicate, they need to be on the **same subnet** so the client can resolve the domain controller's IP address via DNS.

### Resolution
1. **Changed the network adapter for both VMs to Host-Only Adapter**
   - This places both VMs on the same virtual network (`192.168.56.0/24`)
   - The domain controller was assigned `192.168.56.102`
   - The client VM was assigned an IP in the same subnet (e.g., `192.168.56.x`)

2. **Configured static IP on the Domain Controller**
   - IP Address: `192.168.56.102`
   - Subnet Mask: `255.255.255.0`
   - Preferred DNS: `127.0.0.1` (self)

3. **Configured DNS on the Client VM**
   - Preferred DNS: `192.168.56.102` (pointing to the DC)
   - This allowed the client to resolve `ROD.LOCAL` and locate the domain controller

### Verification
- The client successfully joined the domain `ROD.LOCAL`
- The client could ping `192.168.56.102` and resolve `ROD.LOCAL`
- ADUC showed the client computer object (`RODSEC`) in the Computers OU

---

## Organisational Unit (OU) Structure

```
ROD.LOCAL
├── Builtin
├── Computers
├── Domain Controllers
├── Europe
│   ├── Computers
│   ├── Disabled Accounts
│   ├── Service Accounts
│   └── Users
│       ├── Accounting
│       ├── HR
│       └── IT
├── ForeignSecurityPrincipal
├── Managed Service Accounts
└── Users
```

---

## Security Groups Created

| Group Name | Members | Purpose |
| :--- | :--- | :--- |
| #HR_Department | HR users (e.g., Mensa Otabil, Sarah Doe) | Manage HR team access |

---

## Group Policy Objects (GPOs)

| GPO Name | Linked To | Settings Applied |
| :--- | :--- | :--- |
| **Password Policy** | Europe OU | Enforce password history, minimum password length, complexity requirements, maximum password age |
| **Workstation Security** | Europe OU | Screen saver timeout (600 seconds), password protect screen saver |
| **Drive Mapping** | Europe OU | Maps S: drive to `\\RODSERVER\Shared` |
| **Restrict Control Policy** | Europe OU | Disables Control Panel access for users |

### GPO Details

#### Password Policy
- Enforce password history: Enabled
- Maximum password age: 90 days
- Minimum password age: 1 day
- Minimum password length: 12 characters
- Password must meet complexity requirements: Enabled

#### Workstation Security
- Interactive logon: Machine inactivity limit: 600 seconds
- Screen saver timeout: 600 seconds
- Password protect screen saver: Enabled

#### Drive Mapping
- Maps S: drive to `\\RODSERVER\Shared`
- Reconnect at logon: Enabled
- Drive letter: S:

#### Restrict Control Policy
- Disables Control Panel access
- Removes Control Panel from Start menu and File Explorer
- Prevents users from accessing PC settings

---

## Client Domain Join

| Detail | Value |
| :--- | :--- |
| Client VM | Windows 10 Enterprise |
| Computer Name | RodSec |
| Full Computer Name | RodSec.rod.local |
| Domain | rod.local |
| Status | Successfully joined |

### Domain Join Process

1. **Configured DNS** to point to DC01 (`192.168.56.102`)
2. **Joined the domain** using `rod\administrator` credentials
3. **Verified in ADUC** under `Computers` OU
4. **Confirmed** `Full computer name: RodSec.rod.local` in System Properties

---

## User Accounts Created

| Username | Full Name | Department | Title |
| :--- | :--- | :--- | :--- |
| m.otabil | Mensa Otabil | HR | HR Director |
| s.doe | Sarah Doe | HR | HR Officer |
| k.bond | Kevin Bond | IT | IT Manager |

---

## GPO Verification: Restrict Control Panel

To confirm that the Restrict Control Panel GPO was successfully applied, I logged in as a domain user (`ROD\m.otabil`) and attempted to open Control Panel.

### Step 1: User Login
> **Screenshot:** [15-user-login.jpg](screenshots/15-user-login.jpg)
> 
> I logged in as `ROD\m.otabil`, a standard domain user in the HR department. This confirms the user account is active and the domain authentication is working.

### Step 2: Attempt to Open Control Panel
> **Screenshot:** [16-control-panel-attempt.jpg](screenshots/16-control-panel-attempt.jpg)
> 
> From the Start menu, I clicked on Control Panel. This is the action a normal user would take to access system settings.

### Step 3: Access Denied — GPO Enforced
> **Screenshot:** [17-control-panel-blocked.jpg](screenshots/17-control-panel-blocked.jpg)
> 
> Instead of opening Control Panel, the user received this error message:
> 
> > *"This operation has been cancelled due to restrictions in effect on this computer. Please contact your system administrator."*
> 
> **This confirms that the Restrict Control Panel GPO is successfully enforced.** The policy is working as intended — standard users cannot access Control Panel, protecting the system from unauthorized changes.

---

## Screenshots

### Network Configuration
| File Name | Description |
| :--- | :--- |
| [dc-network-adapter-settings.jpg](screenshots/dc-network-adapter-settings.jpg) | Domain controller VirtualBox adapter settings (Host-Only) |
| [client-network-adapter-settings.jpg](screenshots/client-network-adapter-settings.jpg) | Client VM VirtualBox adapter settings (Host-Only) |
| [dc-static-ip.jpg](screenshots/dc-static-ip.jpg) | Domain controller static IP configuration (`192.168.56.102`) |
| [client-dns-settings.jpg](screenshots/client-dns-settings.jpg) | Client DNS pointing to domain controller (`192.168.56.102`) |

### Active Directory Setup
| File Name | Description |
| :--- | :--- |
| [01-vm-running.jpg](screenshots/01-vm-running.jpg) | VirtualBox showing DC01-TechBridge running |
| [02-server-manager-welcome.jpg](screenshots/02-server-manager-welcome.jpg) | Server Manager Dashboard after initial setup |
| [03-add-roles-and-features.jpg](screenshots/03-add-roles-and-features.jpg) | Adding AD DS and DNS Server roles |
| [04-ad-ds-role-selected.jpg](screenshots/04-ad-ds-role-selected.jpg) | AD DS role selected in installation wizard |
| [05-domain-controller-promotion.jpg](screenshots/05-domain-controller-promotion.jpg) | Promoting server to domain controller (ROD.LOCAL) |
| [06-aduc-ou-structure.jpg](screenshots/06-aduc-ou-structure.jpg) | Active Directory Users and Computers showing OU tree |
| [07-group-properties.jpg](screenshots/07-group-properties.jpg) | #HR_Department group properties with members |
| [08-gpo-password-policy.jpg](screenshots/08-gpo-password-policy.jpg) | Password Policy GPO settings |
| [09-gpo-workstation-security.jpg](screenshots/09-gpo-workstation-security.jpg) | Workstation Security GPO settings |
| [10-gpo-drive-mapping.jpg](screenshots/10-gpo-drive-mapping.jpg) | Drive Mapping GPO configuration |
| [11-gpo-restrict-control-policy.jpg](screenshots/11-gpo-restrict-control-policy.jpg) | Restrict Control Panel GPO settings |
| [12-client-domain-join.jpg](screenshots/12-client-domain-join.jpg) | Windows 10 joining ROD.LOCAL domain |
| [13-client-system-properties.jpg](screenshots/13-client-system-properties.jpg) | Windows 10 showing `RodSec.rod.local` |
| [14-aduc-client-verified.jpg](screenshots/14-aduc-client-verified.jpg) | ADUC showing RODSEC computer object |

### GPO Verification
| File Name | Description |
| :--- | :--- |
| [15-user-login.jpg](screenshots/15-user-login.jpg) | Domain user (ROD\m.otabil) logging into Windows 10 client |
| [16-control-panel-attempt.jpg](screenshots/16-control-panel-attempt.jpg) | User attempting to open Control Panel from the Start menu |
| [17-control-panel-blocked.jpg](screenshots/17-control-panel-blocked.jpg) | Error message: "This operation has been cancelled due to restrictions" — GPO successfully enforced |

---

## Troubleshooting Encountered

### Issue: Client Could Not Resolve Domain
- **Cause:** DNS not pointing to domain controller
- **Resolution:** Set preferred DNS to `192.168.56.102` (DC01 IP)

### Issue: VMs Could Not Communicate (NAT Adapter)
- **Cause:** NAT adapter in VirtualBox creates an isolated network that does not allow VM-to-VM communication
- **Resolution:** Changed both VMs to **Host-Only Adapter** and assigned static IP addresses on the same subnet (`192.168.56.0/24`)

### Issue: GPO Not Applying to Users
- **Cause:** GPO linked at domain level, not OU level
- **Resolution:** Linked GPOs specifically to `Europe` OU

---

## Key Learnings

- Active Directory is the foundation of identity management in Windows environments
- Group Policy Objects allow centralized management of user and computer settings
- Proper OU structure is critical for scalable management
- DNS configuration is essential for domain join success
- Security groups simplify permission management
- **Network configuration (Host-Only vs. NAT) is critical for VM-to-VM communication**
- **Verifying GPO enforcement is just as important as configuring it**

---

## Future Improvements

- Add more department OUs and security groups
- Implement additional GPOs for software deployment
- Set up folder redirection for user profiles
- Integrate with Microsoft 365 (simulated)

---

## Project Files

| File | Description |
| :--- | :--- |
| `screenshots/` | All screenshots from the lab setup |
| `README.md` | This documentation |

---

## Repository Link

[View this project on GitHub](https://github.com/yourusername/it-support-portfolio/tree/main/project-1-active-directory)