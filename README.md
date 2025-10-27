# IT Pre-Onboarding Runbook

**Runbook:** IT Pre-Onboarding Process for New Hires  
**Project:** IT Pre-Onboarding Runbook — UNF / StackFull

---

## Project Overview
This runbook documents step-by-step IT pre-onboarding procedures to prepare a new hire’s workstation, user account, and network access before their first day. The document focuses on repeatable, secure, and auditable actions for domain-joined environments, Active Directory (AD), group policy (GPO), file shares, and basic PowerShell checks.

---

## Objective
Provide a consistent, secure, and efficient process for provisioning new employees, reducing setup errors and accelerating time-to-productivity. Tasks covered include domain join, AD user and group creation, OU placement, GPO application, share mapping, and basic verification scripts.

---

## Key Deliverables
- Domain join instructions
- AD user creation and password policy
- Department group creation and membership
- Departmental file share creation with NTFS & share permissions
- Organizational Unit (OU) creation and object movement
- GPO creation and linking (login scripts, login message, restrictions)
- PowerShell scripts for post-provision verification (installed software, running services)
- Checklist for final verification and handoff

---

## High-Level Process
1. Join computer to domain (`contoso.com`).  
2. Create AD user account and enforce password change at first logon.  
3. Create department security group and add the new user.  
4. Create departmental file share and set NTFS and share permissions.  
5. Create or select an Organizational Unit (OU); move computer and user objects into the OU.  
6. Create or link GPOs to the OU to enforce required settings (MFA reminders, login scripts, blocked apps).  
7. Run verification steps (Event Viewer, PowerShell checks).  
8. Record completion and notify manager.

---

## Tools & Prerequisites
- Windows Server / Active Directory management tools (ADUC, GPMC)  
- PowerShell (Windows PowerShell 5.1+ / PowerShell Core)  
- Admin credentials with rights to create users/groups, edit GPOs, and manage file shares  
- File server with NTFS and share management access

---

