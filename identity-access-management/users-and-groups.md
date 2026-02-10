## Identity Provisioning: Users and Security Groups

# Creates Users via Microsoft Graph

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

 - Created an enabled account
 - Assigned department attribute
 - Forced password change at first sign in

# Created Department Security Group

New-MgGroup -DisplayName "SG-Finance-Users" -MailEnabled:$false -MailNickname "sgfinanceusers" -SecurityEnabled:$true
New-MgGroup -DisplayName "SG-HR-Users" -MailEnabled:$false -MailNickname "sghrusers" -SecurityEnabled:$true
New-MgGroup -DisplayName "SG-IT-Users" -MailEnabled:$false -MailNickname "sgitusers" -SecurityEnabled:$true
New-MgGroup -DisplayName "SG-Sales-Users" -MailEnabled:$false -MailNickname "sgsalesusers" -SecurityEnabled:$true

- Naming conventions based on deapartment
- Security groups via M365 groups


# DeviceCodeCredentials Error

- updated from PowerShell 5.1.26100.7705 to PowerShell 7
- winget install Microsoft.PowerShell


# Automated User Creation 

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
- .csv file received from the HR department
- reduces manual errors
- easier RBAC assignment 
