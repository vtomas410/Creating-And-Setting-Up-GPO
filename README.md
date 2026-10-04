# Group Policy Management

## Project Overview

In this phase of the Windows Server home lab, I configured **Group Policy Management** and created several Group Policy Objects (GPOs) within the Active Directory domain.

The purpose of this lab was to practice using Group Policy to centrally manage security settings, user configurations, computer configurations, and access restrictions.

---

# 1. Prerequisites

Before beginning this lab, the following were already configured:

- VMware Workstation Pro
- Windows Server
- Active Directory Domain Services
- Domain Controller
- Active Directory domain
- Organizational Units
- Active Directory users and groups

The server must also have the **Group Policy Management Console (GPMC)** available.

---

# 2. Install Group Policy Management

1. Open **Server Manager**.
2. Select **Manage**.
3. Select **Add Roles and Features**.
4. Continue through the installation wizard.
5. Select the appropriate server.
6. Navigate to **Features**.
7. Locate the Group Policy Management tools.
8. Select **Group Policy Management**.
9. Continue through the wizard.
10. Select **Install**.
11. Wait for the installation to finish.

After installation, Group Policy Management can be accessed through:

**Server Manager → Tools → Group Policy Management**

---

# 3. Open Group Policy Management

1. Open **Server Manager**.
2. Select **Tools**.
3. Select **Group Policy Management**.
4. Expand the Active Directory forest.
5. Expand the domain.
6. Review the available Organizational Units and existing Group Policy Objects.

The Group Policy Management Console provides a centralized location for creating, editing, linking, and managing GPOs.

---

# 4. Understanding Group Policy Objects

A **Group Policy Object (GPO)** is a collection of settings that can be applied to users and computers within an Active Directory environment.

GPOs can be used to control things such as:

- Password requirements
- Account lockout settings
- Desktop settings
- Control Panel access
- USB storage access
- Network drive mappings
- Security settings
- Administrative restrictions

---

# 5. Computer Configuration vs. User Configuration

When creating a GPO, there are two primary configuration sections.

### Computer Configuration

Computer Configuration applies settings to the computer itself.

These settings remain associated with the computer regardless of which user logs in.

Examples:

- USB restrictions
- Computer security settings
- Software configuration
- System restrictions

### User Configuration

User Configuration applies settings to individual user accounts.

These settings follow the user when they log into a domain computer.

Examples:

- Desktop wallpaper
- Network drive mappings
- Control Panel restrictions
- User interface settings

---

# 6. Policies vs. Preferences

Within Computer Configuration and User Configuration, Group Policy contains both **Policies** and **Preferences**.

### Policies

Policies are used when administrators need to enforce a setting.

Users generally cannot override an enforced policy.

Examples:

- Password requirements
- Account restrictions
- Control Panel restrictions
- USB restrictions

### Preferences

Preferences are used when administrators want to provide or configure default settings while allowing more flexibility.

Example:

- Mapping a network drive
- Creating shortcuts
- Configuring certain user settings

---

# 7. GPO #1 — Password Policy

The first GPO created was a password policy.

### Create the GPO

1. Open **Group Policy Management**.
2. Right-click the domain.
3. Select **Create a GPO in this domain, and Link it here**.
4. Name the GPO:

`Password Policy`

5. Right-click the new GPO.
6. Select **Edit**.

### Configure Password Requirements

Navigate to:

**Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy**

Configure settings such as:

- Minimum password length
- Password complexity requirements
- Maximum password age

Example configuration:

```text
Minimum password length: 12 characters
Password complexity: Enabled
Maximum password age: 90 days
```

The purpose of this GPO is to enforce stronger password requirements across the domain.

---

# 8. GPO #2 — Drive Mapping

The second GPO was created to automatically map a network drive for users.

### Create the GPO

1. Create a new GPO.
2. Name it:

`Drive Mapping`

3. Edit the GPO.

Because the drive is associated with the user, use **User Configuration**.

Navigate to:

**User Configuration → Preferences → Windows Settings → Drive Maps**

4. Right-click **Drive Maps**.
5. Select **New → Mapped Drive**.
6. Select the desired drive letter.

Example:

`E:`

7. Enter the network share path.

Example:

```text
\\ServerName\SharedFolder
```

8. Apply the configuration.

When the appropriate user logs into a domain computer, the network drive can automatically appear.

---

# 9. GPO #3 — Desktop Wallpaper

The third GPO was created to control the desktop wallpaper for users.

### Create the GPO

1. Create a new GPO.
2. Name it:

`Desktop Wallpaper`

3. Edit the GPO.

Navigate through:

**User Configuration → Policies → Administrative Templates**

Locate the desktop-related wallpaper settings.

4. Enable the desktop wallpaper policy.
5. Specify the location of the wallpaper image.
6. Select an appropriate wallpaper style.

Example:

```text
Wallpaper Style: Fill
```

This allows the administrator to centrally control the desktop background for users assigned to the GPO.

---

# 10. GPO #4 — Restrict Control Panel

The fourth GPO was created to prevent users from accessing the Control Panel and PC settings.

### Create the GPO

1. Create a new GPO.
2. Name it:

`Restrict Control Panel`

3. Edit the GPO.

Navigate to:

**User Configuration → Policies → Administrative Templates → Control Panel**

4. Locate the setting that prevents access to Control Panel and PC settings.
5. Enable the policy.
6. Apply the changes.

This can be used to prevent standard users from changing system configuration settings.

---

# 11. GPO #5 — Restrict USB Storage Devices

The fifth GPO was created to restrict removable storage devices.

Because this restriction applies to the computer, **Computer Configuration** is used.

### Create the GPO

1. Create a new GPO.
2. Name it:

`USB Devices`

3. Edit the GPO.

Navigate to:

**Computer Configuration → Policies → Administrative Templates → System → Removable Storage Access**

4. Locate the appropriate removable-storage restrictions.
5. Enable the required restriction.
6. Apply the changes.

This type of GPO can help prevent unauthorized use of USB storage devices.

---

# 12. Account Lockout Policy — Bonus Activity

An additional exercise is configuring an account lockout policy.

Account lockout policies help protect against repeated password-guessing attempts.

Important settings include:

- Account lockout threshold
- Account lockout duration
- Reset account lockout counter after

Example:

```text
Account lockout threshold: 5 failed attempts
Account lockout duration: 15 minutes
Reset counter after: 15 minutes
```

These values are examples for the lab and can be adjusted depending on the organization's security requirements.

---

# 13. Apply and Test GPOs

After creating the GPOs, they should be tested on a domain-connected client computer.

On the client machine, open Command Prompt and run:

```cmd
gpupdate /force
```

This forces the computer to refresh its Group Policy settings.

To view applied policies, use:

```cmd
gpresult /r
```

For user-specific policies:

```cmd
gpresult /r /scope:user
```

For computer-specific policies:

```cmd
gpresult /r /scope:computer
```

---

# 14. Verify GPO Application

After refreshing Group Policy, verify that the expected settings have been applied.

Examples:

### Password Policy

Verify that the domain password requirements are being enforced.

### Drive Mapping

Check whether the expected network drive appears.

### Desktop Wallpaper

Verify that the configured wallpaper is applied.

### Control Panel

Verify that standard users cannot access the restricted settings.

### USB Restrictions

Test whether removable storage access is restricted as configured.

---

# 15. Troubleshooting

If a GPO does not apply correctly, check the following:

1. Verify that the GPO is linked to the correct domain or OU.
2. Verify that the user or computer is located in the correct OU.
3. Run:

```cmd
gpupdate /force
```

4. Run:

```cmd
gpresult /r
```

5. Check the GPO's **Scope** section in Group Policy Management.
6. Verify Security Filtering.
7. Confirm that the client computer is connected to the domain.
8. Review Group Policy-related events in Event Viewer if necessary.

---

# 16. Lab Structure

The completed environment can be organized similar to:

```text
Active Directory Domain
│
├── Domain Controllers
│
├── Users
│
├── Computers
│
├── IT
│
├── HR
│
└── Groups
```

Example GPOs:

```text
Group Policy Objects
│
├── Password Policy
├── Drive Mapping
├── Desktop Wallpaper
├── Restrict Control Panel
└── USB Devices
```

---

# 17. Skills Demonstrated

This lab demonstrates practical experience with:

- Group Policy Management
- Group Policy Objects
- Active Directory
- Windows Server
- Computer Configuration
- User Configuration
- Security Policies
- Password Policies
- Account Lockout Policies
- Network Drive Mapping
- Administrative Templates
- Removable Storage Restrictions
- Group Policy troubleshooting
- `gpupdate`
- `gpresult`
- Windows Server administration

---

# Project Outcome

After completing this lab, I was able to create and manage multiple Group Policy Objects within my Active Directory environment.

The lab provided hands-on experience with centrally managing user and computer configurations and demonstrated how Group Policy can be used to implement security controls and administrative restrictions across a Windows domain.
