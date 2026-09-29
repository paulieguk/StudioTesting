# Content from Github

---

<center>
#Applied Skills UI Change Preview Lab

</center>

---

This lab highligths the following lab client changes

- Enhanced Pre-Instanced Lab Resume page now stating Lauching and showing some Lab Guidance
- <span class="material-symbols-outlined" style="font-size: 1em; aria-hidden="true">select_window</span> Instruction Pop Out Icon
- <span class="material-symbols-outlined" style="font-size: 1em; aria-hidden="true">right_panel_close</span> Instruction Collapse Icon
- **Help** menu in the lab Instructions (above), contains new help categories:
    - User Guides
    - Troubleshooting
    - FAQ
    - Contact Support


===

<!-- AI generated content 1788447682515 -->
===

# Lab: Creating User Accounts in Windows

## Objective

In this lab, you will create, manage, and validate local user accounts in Windows 11. You will use both graphical tools and command-line tools to understand how Windows stores and controls local users, groups, passwords, and account permissions.

By the end of this lab, you should be able to:

- Create local user accounts using **Settings**
- Create and manage users using **Computer Management**
- Create users using **PowerShell**
- Create users using **Command Prompt**
- Add users to local groups
- Understand the difference between **standard users** and **administrators**
- Configure password and account options
- Test user sign-in behavior
- Disable, enable, rename, and delete local accounts

---

## Estimated Time

45-60 minutes

---

## Prerequisites

You should have access to a Windows 11 desktop environment with an account that has local administrator permissions.

> [!note]  
> Some Windows 11 editions hide or limit certain account management tools. For example, **Local Users and Groups** is typically available in Windows 11 Pro, Enterprise, and Education, but not in Windows 11 Home.

---

## Lab Scenario

You are preparing a shared Windows workstation for multiple users. You need to create local accounts for different roles:

| User | Purpose | Account Type |
|---|---|---|
| `labstudent` | Regular daily user | Standard user |
| `labadmin2` | Backup administrative account | Administrator |
| `tempuser` | Temporary short-term access | Standard user |

You will create these accounts using different Windows management methods and then verify how permissions differ between them.

---

# Part 1: Review Existing Local Accounts

## Step 1: Open Computer Management

1. Right-click the **Start** button.
2. Select **Computer Management**.
3. In the left pane, expand:

   **System Tools > Local Users and Groups > Users**

4. Review the list of existing local user accounts.

> [!hint]  
> If you do not see **Local Users and Groups**, use the PowerShell steps later in the lab instead.

---

## Step 2: Identify Built-in Accounts

Look for accounts such as:

- `Administrator`
- `DefaultAccount`
- `Guest`
- `WDAGUtilityAccount`

These are built-in or system-managed accounts.

> [!knowledge]  
> The built-in `Administrator` account is disabled by default on modern Windows systems. It has elevated privileges but is not normally used for daily administration. Windows encourages using a named administrator account instead, which improves auditing and accountability.

---

# Part 2: Create a Local User Using Windows Settings

## Step 1: Open Account Settings

1. Open **Settings**.
2. Select **Accounts**.
3. Select **Other users**.

## Step 2: Add a Local User

1. Under **Other users**, select **Add account**.
2. If prompted for a Microsoft account, select:

   **I don't have this person's sign-in information**

3. Select:

   **Add a user without a Microsoft account**

4. Enter the following information:

   - **User name:** `labstudent`
   - **Password:** `P@ssw0rd-Lab1`
   - **Security questions:** Choose any answers suitable for the lab

5. Select **Next**.

> [!alert]  
> Do not use a real personal password in lab environments. Use only temporary lab passwords that can be changed or removed later.

---

## Step 3: Verify the Account Type

1. In **Settings > Accounts > Other users**, locate `labstudent`.
2. Expand the account entry.
3. Confirm it is listed as a **Local account**.
4. Confirm it is a **Standard User**.

> [!note]  
> A standard user can run most applications and change personal settings, but cannot install system-wide software, modify protected system files, or manage other users without administrator approval.

---

# Part 3: Create a Local User Using Computer Management

## Step 1: Open the Users Folder

1. Right-click **Start**.
2. Select **Computer Management**.
3. Go to:

   **System Tools > Local Users and Groups > Users**

## Step 2: Create the User

1. Right-click **Users**.
2. Select **New User**.
3. Enter:

   - **User name:** `tempuser`
   - **Full name:** `Temporary User`
   - **Description:** `Temporary local account for lab testing`
   - **Password:** `P@ssw0rd-Temp1`
   - **Confirm password:** `P@ssw0rd-Temp1`

4. Clear the checkbox:

   **User must change password at next logon**

5. Select the checkbox:

   **Password never expires**

6. Select **Create**.
7. Select **Close**.

> [!alert]  
> In production environments, avoid using **Password never expires** unless there is a specific policy-based reason. Accounts with non-expiring passwords increase security risk.

---

## Step 3: Review User Properties

1. Double-click `tempuser`.
2. Review the tabs available, such as:
   - **General**
   - **Member Of**
   - **Profile**

3. Select the **Member Of** tab.

By default, `tempuser` should be a member of the **Users** group.

> [!knowledge]  
> Local group membership controls what a user can do on the computer. The most important distinction is usually between the **Users** group and the **Administrators** group. Members of **Administrators** can elevate privileges through User Account Control when administrative tasks are required.

---

# Part 4: Create a Local Administrator Using PowerShell

## Step 1: Open PowerShell as Administrator

1. Right-click **Start**.
2. Select **Terminal (Admin)**.
3. If prompted by User Account Control, select **Yes**.
4. Confirm the terminal opens with an elevated prompt.

> [!hint]  
> In Windows 11, Terminal may open PowerShell by default. If it opens Command Prompt instead, select the drop-down arrow in Windows Terminal and choose **Windows PowerShell** or **PowerShell**.

---

## Step 2: Create a Secure Password Variable

Run the following command:

```powershell
$Password = Read-Host "Enter password for labadmin2" -AsSecureString
```

When prompted, enter:

```text
P@ssw0rd-Admin2
```

The password will not appear as you type.

---

## Step 3: Create the User

Run:

```powershell
New-LocalUser -Name "labadmin2" `
  -FullName "Lab Backup Administrator" `
  -Description "Backup local administrator account for lab" `
  -Password $Password
```

---

## Step 4: Add the User to the Administrators Group

Run:

```powershell
Add-LocalGroupMember -Group "Administrators" -Member "labadmin2"
```

---

## Step 5: Verify the Account

Run:

```powershell
Get-LocalUser -Name "labadmin2"
```

Then verify group membership:

```powershell
Get-LocalGroupMember -Group "Administrators"
```

You should see `labadmin2` listed as a member of the local **Administrators** group.

> [!alert]  
> Adding a user to the **Administrators** group gives that account significant control over the computer. Only assign administrative rights when required.

---

# Part 5: Create a User Using Command Prompt

## Step 1: Open Command Prompt as Administrator

1. Open **Start**.
2. Type:

   ```text
   cmd
   ```

3. Select **Run as administrator**.
4. If prompted by User Account Control, select **Yes**.

---

## Step 2: Create a Standard User

Run:

```cmd
net user cmduser P@ssw0rd-Cmd1 /add
```

---

## Step 3: Add a Full Name and Comment

Run:

```cmd
net user cmduser /fullname:"Command Line User" /comment:"Created using net user command"
```

---

## Step 4: Verify the User

Run:

```cmd
net user cmduser
```

Review the output for:

- User name
- Full name
- Account active
- Password last set
- Local group memberships

> [!knowledge]  
> The `net user` command is older than PowerShell but is still widely used for quick local account tasks, scripting, and troubleshooting. PowerShell provides more structured output and is often easier to automate in modern Windows environments.

---

# Part 6: Compare Standard and Administrator Accounts

## Step 1: Check Group Memberships

Open PowerShell as Administrator and run:

```powershell
Get-LocalUser | Select-Object Name, Enabled, LastLogon
```

Then run:

```powershell
Get-LocalGroupMember -Group "Users"
```

And:

```powershell
Get-LocalGroupMember -Group "Administrators"
```

Record which accounts are standard users and which are administrators.

---

## Step 2: Attempt an Administrative Task as a Standard User

1. Sign out of the current account.
2. Sign in as:

   - **Username:** `labstudent`
   - **Password:** `P@ssw0rd-Lab1`

3. Open **Settings**.
4. Try to change another user's account type.

You should be prompted for administrator credentials or blocked from completing the task.

> [!hint]  
> If the lab environment does not allow switching users, you can still inspect account permissions by checking local group membership from an elevated PowerShell session.

---

## Step 3: Attempt an Administrative Task as `labadmin2`

1. Sign out.
2. Sign in as:

   - **Username:** `labadmin2`
   - **Password:** `P@ssw0rd-Admin2`

3. Open **Computer Management** or **Settings > Accounts > Other users**.
4. Confirm you can manage other local users.

> [!note]  
> Even administrator accounts do not run every process with full administrative rights automatically. User Account Control separates standard user privileges from elevated administrator privileges until approval is given.

---

# Part 7: Modify User Account Properties

## Step 1: Disable a Temporary Account

Using PowerShell as Administrator, run:

```powershell
Disable-LocalUser -Name "tempuser"
```

Verify:

```powershell
Get-LocalUser -Name "tempuser"
```

The **Enabled** value should show `False`.

---

## Step 2: Re-enable the Account

Run:

```powershell
Enable-LocalUser -Name "tempuser"
```

Verify:

```powershell
Get-LocalUser -Name "tempuser"
```

The **Enabled** value should show `True`.

---

## Step 3: Rename a User Account

Rename `cmduser` to `scriptuser`:

```powershell
Rename-LocalUser -Name "cmduser" -NewName "scriptuser"
```

Verify:

```powershell
Get-LocalUser -Name "scriptuser"
```

> [!knowledge]  
> Renaming a local user changes the displayed username, but the account's Security Identifier, or SID, remains the same. Windows uses the SID internally for permissions. This is why renamed users may still retain access to resources they previously had.

---

## Step 4: Change a User Password

Run:

```powershell
$NewPassword = Read-Host "Enter new password for tempuser" -AsSecureString
Set-LocalUser -Name "tempuser" -Password $NewPassword
```

When prompted, enter:

```text
P@ssw0rd-Temp2
```

---

# Part 8: Configure Local Group Membership

## Step 1: Create a Custom Local Group

Run PowerShell as Administrator:

```powershell
New-LocalGroup -Name "LabOperators" -Description "Custom group for lab account testing"
```

---

## Step 2: Add Users to the Group

Run:

```powershell
Add-LocalGroupMember -Group "LabOperators" -Member "labstudent","tempuser"
```

---

## Step 3: Verify Membership

Run:

```powershell
Get-LocalGroupMember -Group "LabOperators"
```

---

## Step 4: Remove a User from the Group

Run:

```powershell
Remove-LocalGroupMember -Group "LabOperators" -Member "tempuser"
```

Verify:

```powershell
Get-LocalGroupMember -Group "LabOperators"
```

`tempuser` should no longer appear in the group.

> [!note]  
> Custom groups are useful when assigning permissions to files, folders, printers, or applications. Instead of granting access to individual users, grant access to a group and manage membership centrally.

---

# Part 9: Inspect Accounts with Local Security Policy

## Step 1: Open Local Security Policy

1. Press **Windows key + R**.
2. Type:

   ```text
   secpol.msc
   ```

3. Press **Enter**.

## Step 2: Review Account Policies

Navigate to:

**Account Policies > Password Policy**

Review settings such as:

- Enforce password history
- Maximum password age
- Minimum password age
- Minimum password length
- Password must meet complexity requirements

---

## Step 3: Review Account Lockout Policy

Navigate to:

**Account Policies > Account Lockout Policy**

Review settings such as:

- Account lockout duration
- Account lockout threshold
- Reset account lockout counter after

> [!alert]  
> Local password and lockout policies affect local accounts on the machine. In a domain environment, domain policy may override local policy.

---

# Part 10: Cleanup

Remove the lab accounts and custom group if you do not need them after the lab.

## Step 1: Remove Lab Users

Open PowerShell as Administrator and run:

```powershell
Remove-LocalUser -Name "labstudent"
Remove-LocalUser -Name "labadmin2"
Remove-LocalUser -Name "tempuser"
Remove-LocalUser -Name "scriptuser"
```

---

## Step 2: Remove the Custom Group

Run:

```powershell
Remove-LocalGroup -Name "LabOperators"
```

---

## Step 3: Verify Cleanup

Run:

```powershell
Get-LocalUser
Get-LocalGroup
```

Confirm the lab-created accounts and group are no longer present.

> [!alert]  
> Deleting a local user account removes the account object, but it may not automatically remove all profile data from `C:\Users`. Check the user profile folder if storage cleanup is required.

---

# Validation Checklist

Use this checklist to confirm completion:

- [ ] Reviewed existing local users
- [ ] Created `labstudent` using Settings
- [ ] Created `tempuser` using Computer Management
- [ ] Created `labadmin2` using PowerShell
- [ ] Created `cmduser` using Command Prompt
- [ ] Verified user account properties
- [ ] Compared standard and administrator permissions
- [ ] Disabled and enabled a local user
- [ ] Renamed a local user
- [ ] Changed a local user password
- [ ] Created and managed a custom local group
- [ ] Reviewed local password and lockout policies
- [ ] Removed lab-created accounts and groups

---

# Troubleshooting

## Local Users and Groups Is Missing

If **Local Users and Groups** is not available, use PowerShell instead:

```powershell
Get-LocalUser
New-LocalUser
Set-LocalUser
Remove-LocalUser
Get-LocalGroup
Add-LocalGroupMember
Remove-LocalGroupMember
```

This usually occurs on Windows Home editions.

---

## PowerShell Says Access Is Denied

Make sure PowerShell is running as Administrator.

Check the title bar of Windows Terminal. It should indicate an elevated session, or you should have accepted a User Account Control prompt.

---

## Password Does Not Meet Requirements

Use a password that meets Windows complexity rules. A lab-safe example is:

```text
P@ssw0rd-Lab1
```

Common requirements include:

- At least 8 characters
- Uppercase letter
- Lowercase letter
- Number
- Symbol
- Not based on the username

---

## User Does Not Appear at Sign-in Screen

Some local accounts may not appear immediately. Try one of the following:

- Select **Other user**
- Enter the username manually
- Use the format:

```text
.\username
```

Example:

```text
.\labstudent
```

> [!hint]  
> The `.\` prefix tells Windows to authenticate against the local computer instead of a domain or Microsoft account provider.

---

# Useful Reference Links

- [https://learn.microsoft.com/en-us/windows/security/identity-protection/access-control/local-accounts](https://learn.microsoft.com/en-us/windows/security/identity-protection/access-control/local-accounts)
- [https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/new-localuser](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/new-localuser)
- [https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/get-localuser](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.localaccounts/get-localuser)
- [https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/net-user](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/net-user)
- [https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/)
<!-- End AI generated content 1788447682515 -->




@lab.Activity(Automated1)
@lab.Activity(Automated2)

