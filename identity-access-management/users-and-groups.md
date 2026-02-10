## Identity Provisioning: Users and Security Groups
This lab establishes foundational identity structure prior to RBAC implementation.


### 1. Created Users via Microsoft Graph

 - Created an enabled account
 - Assigned department attribute
 - Forced password change at first sign in

New-MgUser `
 -DisplayName $user.DisplayName `
 -UserPrincipalName $user.UserPrincipalName `
 -MailNickname $user.MailNickname `
 -AccountEnabled `
 -Department $user.Department `
 -PasswordProfile @{
     Password = $user.Password
     ForceChangePasswordNextSignIn = $true
 }

### 2. Created Department Security Groups

- Naming conventions based on department
- Created security groups (not Microsoft 365 collaboration groups)


New-MgGroup -DisplayName "SG-Finance-Users" -MailEnabled:$false -MailNickname "sgfinanceusers" -SecurityEnabled:$true
New-MgGroup -DisplayName "SG-HR-Users" -MailEnabled:$false -MailNickname "sghrusers" -SecurityEnabled:$true
New-MgGroup -DisplayName "SG-IT-Users" -MailEnabled:$false -MailNickname "sgitusers" -SecurityEnabled:$true
New-MgGroup -DisplayName "SG-Sales-Users" -MailEnabled:$false -MailNickname "sgsalesusers" -SecurityEnabled:$true


### 3. Troubleshooting: DeviceCodeCredentials Error
- recevied DeviceCodeError while importing users
- updated from PowerShell 5.1.26100.7705 to PowerShell 7
winget install Microsoft.PowerShell


### 4. Automated User Creation 

- .csv file received from the HR department
- Group-based identity structure was implemented to support scalable RBAC assignment and reduce direct user permission management.


$users = @(
    @{First="Alex"; Last="Morgan"; Dept="IT"},
    @{First="Maya"; Last="Rodriguez"; Dept="HR"},
    @{First="Jordan"; Last="Lee"; Dept="Finance"},
    @{First="Taylor"; Last="Nguyen"; Dept="Sales"},
    @{First="Chris"; Last="Walker"; Dept="IT"},
    @{First="Sam"; Last="Patel"; Dept="HR"}
)

foreach ($u in $users) {

    $displayName = "$($u.First) $($u.Last)"
    $upn = "$($u.First).$($u.Last)@$domain"

    New-MgUser `
        -DisplayName $displayName `
        -UserPrincipalName $upn `
        -MailNickname "$($u.First)$($u.Last)" `
        -AccountEnabled:$true `
        -Department $u.Dept `
        -PasswordProfile @{
            Password = "TempPass123!"
            ForceChangePasswordNextSignIn = $true
        }
}

