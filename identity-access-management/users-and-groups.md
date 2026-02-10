## Identity Provisioning: Users and Security Groups

### 1 Created Users via Microsoft Graph

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

### 2 Created Department Security Groups

- Naming conventions based on deapartment
- Security groups via M365 groups

New-MgGroup -DisplayName "SG-Finance-Users" -MailEnabled:$false -MailNickname "sgfinanceusers" -SecurityEnabled:$true
New-MgGroup -DisplayName "SG-HR-Users" -MailEnabled:$false -MailNickname "sghrusers" -SecurityEnabled:$true
New-MgGroup -DisplayName "SG-IT-Users" -MailEnabled:$false -MailNickname "sgitusers" -SecurityEnabled:$true
New-MgGroup -DisplayName "SG-Sales-Users" -MailEnabled:$false -MailNickname "sgsalesusers" -SecurityEnabled:$true


# DeviceCodeCredentials Error
- recevied DeviceCodeError while importing users
- updated from PowerShell 5.1.26100.7705 to PowerShell 7
winget install Microsoft.PowerShell


# Automated User Creation 

- .csv file received from the HR department
- reduces manual errors
- easier RBAC assignment 

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

