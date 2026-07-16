# Jira Service Management - IT Support Lab

## Project Overview
This project demonstrates my ability to manage IT support tickets using Jira Service Management. I simulated two realistic support scenarios covering the full lifecycle of a ticket—from creation to resolution.

## Tools Used
- Jira Service Management (Free Plan)
- Project Template: IT Service Desk

---

## Project Configuration

| Detail | Value |
| :--- | :--- |
| Project Name | TechBridge IT Support |
| Project Key | TIS |
| Template | IT Service Desk |
| Plan | Free (3 agents) |

---

## Screenshots

| File Name | Description |
| :--- | :--- |
| [01-two-tickets-created.jpg](screenshots/01-two-tickets-created.jpg) | Both tickets (TIS-1 and TIS-2) in the Jira queue |
| [02-ticket-01-resolution.jpg](screenshots/02-ticket-01-resolution.jpg) | Resolution comment added to Ticket TIS-1 (Password Reset) |
| [03-ticket-01-email.jpg](screenshots/03-ticket-01-email.jpg) | Email sent to user with temporary password |
| [04-ticket-02-resolution.jpg](screenshots/04-ticket-02-resolution.jpg) | Resolution comment added to Ticket TIS-2 (Onboarding) |
| [05-ticket-02-email.jpg](screenshots/05-ticket-02-email.jpg) | Welcome email sent to the new employee |

---

## Ticket 1: Password Reset Request (TIS-1)

### Scenario
> *"Mensa Otabil, the HR Director, calls the IT Support desk. She's locked out of her account and needs her password reset urgently. She's on a deadline to send payroll reports to the finance team."*

### Ticket Details

| Field | Value |
| :--- | :--- |
| **Issue Type** | Service Request |
| **Summary** | Password Reset Request - Mensa Otabil (HR Director) |
| **Priority** | Low |
| **Reporter** | Mensa Otabil |
| **Status** | Resolved |

### Resolution Steps

1. **Unlocked the user's AD account**
   - Opened Active Directory Users and Computers
   - Located user account (Mensa Otabil)
   - Right-click → Properties → Account tab
   - Unchecked "Account is locked out"

2. **Reset the password**
   - Right-click → Reset Password
   - Set temporary password: `TechBridge@HR2025!`
   - Enabled "User must change password at next logon"
   - Clicked OK

3. **Communicated resolution to user**
   - Email sent with temporary password and instructions

### Resolution Notes

```
I've reset Mensa's password and unlocked her account. She has been notified of the new temporary password and will be prompted to change it at next login. Resolution completed.

Temporary password: TechBridge@HR2025!
```

### Email Sent to User

```
Subject: Password Reset Completed - Mensa Otabil

Hi Mensa,

I've reset your password and unlocked your account.

Temporary password: TechBridge@HR2025!
You will be prompted to change it at your next login.

If you need any assistance, reply to this ticket.

Regards,
TechBridge IT Support

Ticket: TECHBRIDGE-001
```

---

## Ticket 2: New Employee Onboarding (TIS-2)

### Scenario
> *"Daniel Kan is starting as a Sales Representative on Monday. His manager (Michael Boateng) needs him to have access to all Sales systems, including his laptop, email, shared drives, and the CRM. This is part of a monthly onboarding cohort."*

### Ticket Details

| Field | Value |
| :--- | :--- |
| **Issue Type** | Service Request |
| **Summary** | New Employee Onboarding - Daniel Kan (Sales Rep) |
| **Priority** | Medium |
| **Reporter** | Michael Boateng |
| **Status** | Resolved |

### Subtasks Created

| Subtask | Status | Description |
| :--- | :--- | :--- |
| TIS-3 | Done | Provision laptop (hardware + OS) |
| TIS-4 | Done | Join laptop to domain (ROD.LOCAL) |
| TIS-5 | Done | Create AD user account: d.kan |

### Resolution Steps

1. **Provisioned laptop**
   - Installed Windows 10 Enterprise
   - Configured basic settings

2. **Joined laptop to domain**
   - Joined `ROD.LOCAL`
   - Tested authentication

3. **Created AD user account**
   - Username: `d.kan`
   - Department: Sales
   - Security group membership

4. **Configured access**
   - Shared Drive (S:)
   - Sales Team distribution group
   - HubSpot CRM access

5. **Sent Welcome Email**

### Resolution Notes

```
Onboarding for Daniel Kan completed.

Checklist:
- [x] Laptop provisioned
- [x] Domain joined
- [x] AD user created: d.kan
- [x] Security groups assigned
- [x] Shared drive access granted
- [x] Welcome email sent

Status: Completed. Daniel is ready for Day 1.
```

### Welcome Email Sent

```
Subject: Welcome to TechBridge, Daniel Kan

Dear Daniel,

Welcome to TechBridge Solutions! We're thrilled to have you on board.

Here's what you need to know:

1. Your laptop is ready for you to pick up from IT.
2. Your username is: d.kan
3. Your temporary password is: Welcome@TechBridge2025!
   (You'll need to change this at your first login)
4. You have access to:
   - Shared Drive (S:)
   - Sales Team distribution group
   - HubSpot CRM
5. Your orientation is scheduled for Monday at 9:00 AM with IT.

Let me know if you have any questions.

Best,
TechBridge IT Support
```

---

## SLA Tracking

| Ticket | SLA Type | Target | Status |
| :--- | :--- | :--- | :--- |
| TIS-1 | Time to First Response | 16 hours | ✅ Achieved |
| TIS-1 | Time to Resolution | 60 hours | ✅ Achieved |
| TIS-2 | Time to First Response | 12 hours | ✅ Achieved |
| TIS-2 | Time to Resolution | 48 hours | ✅ Achieved |

---

## Key Learnings

- **Jira Service Management** is a powerful ITSM tool for tracking support requests
- **Ticket categorization** (Priority, Issue Type) helps with SLA tracking
- **Subtasks** are essential for complex, multi-step requests like onboarding
- **Documentation** of resolution steps is critical for knowledge sharing
- **Communication** with the requester is part of the ticket lifecycle
- **Email confirmations** provide proof of resolution to users

---

## Jira Configuration Summary

| Setting | Value |
| :--- | :--- |
| Project Template | IT Service Desk |
| Issue Types | Incident, Service Request, Problem |
| Priorities | High, Medium, Low |
| SLA Tracking | Time to first response, Time to resolution |
| Workflow | Created → In Progress → Resolved → Closed |

---

## Project Files

| File | Description |
| :--- | :--- |
| `screenshots/` | All Jira ticket screenshots |
| `README.md` | This documentation |

---

## Repository Link

[View this project on GitHub](https://github.com/yourusername/it-support-portfolio/tree/main/project-2-jira-ticketing)