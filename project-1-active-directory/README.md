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
| IP Address | 10.0.2.15 (NAT) |
| Virtualization Platform | Oracle VirtualBox |

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

## Verification Steps Performed

| Test | Result |
| :--- | :--- |
| Domain Controller Status | ✅ Healthy |
| DNS Resolution | ✅ Working |
| ADUC shows OU Structure | ✅ Verified |
| User Account Creation | ✅ Successful |
| Security Group Membership | ✅ Verified |
| GPO Application | ✅ Applied to Europe OU |
| Client Domain Join | ✅ Successful |
| Client DNS Resolution | ✅ Verified |

---

## Screenshots

| File Name | Description |
| :--- | :--- |
| `01-vm-running.png` | VirtualBox showing DC01-TechBridge running |
| `02-server-manager-welcome.png` | Server Manager Dashboard after initial setup |
| `03-add-roles-and-features.png` | Adding AD DS and DNS Server roles |
| `04-ad-ds-role-selected.png` | AD DS role selected in installation wizard |
| `05-domain-controller-promotion.png` | Promoting server to domain controller (ROD.LOCAL) |
| `06-aduc-ou-structure.png` | Active Directory Users and Computers showing OU tree |
| `07-group-properties.png` | #HR_Department group properties with members |
| `08-gpo-password-policy.png` | Password Policy GPO settings |
| `09-gpo-workstation-security.png` | Workstation Security GPO settings |
| `10-gpo-drive-mapping.png` | Drive Mapping GPO configuration |
| `11-client-domain-join.png` | Windows 10 joining ROD.LOCAL domain |
| `12-client-system-properties.png` | Windows 10 showing `RodSec.rod.local` |
| `13-aduc-client-verified.png` | ADUC showing RODSEC computer object |

---

## Troubleshooting Encountered

### Issue: Client Could Not Resolve Domain
- **Cause:** DNS not pointing to domain controller
- **Resolution:** Set preferred DNS to `192.168.56.102` (DC01 IP)

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