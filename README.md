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

## Example Commands & Scripts

### Join a Windows machine to the domain (example GUI steps)
1. Open **System Properties** → **Change settings** → **Change**.  
2. Select **Domain**, enter `contoso.com`, click OK.  
3. Provide domain admin credentials when prompted.  
4. Restart the machine.

### Create AD user (PowerShell example)
```powershell
Import-Module ActiveDirectory

New-ADUser -Name "John Doe" -GivenName "John" -Surname "Doe" `
 -SamAccountName "jdoe" -UserPrincipalName "jdoe@contoso.com" `
 -Path "OU=Employees,DC=contoso,DC=com" -AccountPassword (ConvertTo-SecureString "TempP@ssw0rd" -AsPlainText -Force) `
 -Enabled $true -ChangePasswordAtLogon $true
Create security group and add user
New-ADGroup -Name "HR-Staff" -GroupScope Global -Path "OU=Groups,DC=contoso,DC=com"
Add-ADGroupMember -Identity "HR-Staff" -Members "jdoe"

Create and share department folder (example steps)

On file server: C:\Shares\HR → create folder.

Right-click → Properties → Sharing → Advanced Sharing → Share name HR.

Configure share permissions (e.g., HR-Staff → Change, Domain Admins → Full).

Configure NTFS security: HR-Staff → Modify (as needed), Domain Computers → Read & Execute (if required).

Login script to map share (HR-SCRIPT.bat)
@echo off
net use Z: \\fileserver\HR /persistent:no

PowerShell: find most recently installed application
Get-WmiObject -Class Win32_Product | 
  Sort-Object -Property InstallDate -Descending |
  Select-Object -Property Name, InstallDate -First 5

PowerShell: list running services and output to file
Get-Service | Where-Object {$_.Status -eq 'Running'} | 
  Select-Object Name, DisplayName, Status | 
  Out-File -FilePath C:\Temp\running_services.txt -Encoding UTF8

GPO Recommendations (examples)

Enforce MFA reminders and document how to enroll.

Disable Command Prompt for non-admin users if required.

Configure login banner (legal notice) and login script for share mapping.

Prevent use of Run or restrict Task Manager as policy requirements mandate.

Apply Baseline Security Templates (password length, lockout threshold, audit policy).

Verification Checklist

 Computer joined to contoso.com and restarted.

 AD user created with ChangePasswordAtLogon = true.

 User added to department group.

 Departmental share created and permissions set.

 Computer and user moved to target OU.

 GPO linked and applied; gpupdate /force completed.

 Login script maps required shares.

 PowerShell checks (installed apps & running services) executed and recorded.

 Final handoff email sent to manager with credentials instructions.

Security Best Practices

Require a strong temporary password and force change at first logon.

Use AD security groups for access management rather than per-user assignments.

Test GPOs in a lab OU before applying in production.

Keep a secure repository for scripts and restrict execution/modify permissions.

Enable event logging and forward critical events to SIEM where available.
