# Using PowerShell for Active Directory
## Ashley McGlone
## Jason Helmick

# Introduction to Active Directory
- We're going to need the **Remote System Administration Tools**.
- The *AD Web Service* is what allows the remote commands to work.
- The default port is 9389.
- Server Manager runs on port 5985.
- LDP is 389.
- You can remote to a different computer using `New-PSSession`.
- You're going to want to `Import-Module PSSession`.
- This is just using shortcuts that point to the files on the Domain Controller.
- Then you can issues command to it.
- **Active Directory Administrative Center** is supposed to replace ADUC.
- It just runs Powershell underneath it.
- You can change into the *Active Directory Privider* using `ad:`.
- There are short versions of cmdlets:
  * `-S` for Select-Object
  * `-I` for Input-?
  * `-R` for Reset.

# Working With Users and Groups
- We're going to focus on:
  1. Creating
  2. Updating
  3. Deleting
- **Active Directory Service Interface** should die in a fire, I guess.
- An alias for `Get-Member` is `gm`.
- AD Accounts created in AD using Powershell are disabled by default.
- `Import-Csv` and `Export-Csv` are things one should know.
- The backtick is a *line continuation character*.
- You can add a new OU using `New-ADOrganizationalUnit NewUsers`.
- You can force a user to change their password by passing `-ChangePasswordAtLogin` to `New-ADUser`.
- You can update credentials across Domains.
- You can now use `Get-ADForest` to collect all the possible domain names.
- Then, you can use the attribute `Domains` to get all of them.
- You can run the command as a test without affecting anything by appending `-WhatIf`.
- When you run any sort of `Remove` verb it will auto-prompt you to confirm it.
- You can override all of the safety features by appending `-Confirm:$false`.
- There is an explicit `-Recursive` parameter.

# Quering AD Data
- You can see the help using `help about_act`.
- LDAP is not a Microsoft product.
- There are different **Naming Partitions** or **Naming Contexts**.
- CN stands for **Common Name** or **Canonical Name**.
- OU stands for **Organizational Units**.
- DC stands for **Domain Component**.
- `[ADSISEARCHER]` or `[ADSI]` is your interfacing class.
- SID stands for **Security Identifier**.
- You can make queries using `Get-ADUser -Identity` and then:
  * `<acct_name>`
  * `<SID>`
  * `GUID`
  * `<CN=<x>,OU=<y>,...`
- Interesting trick: `whoami /user /fo csv`.
- **DO NOT USE FILTER * PROPERTY**
- To get the Root of the **Domain Service Tree** use `Get-ADRootDSE`.
- To get the list of subdomains use `Get-ADDomain`.
- To get the list of Domains use `Get-ADForest`.
- You can allow PowerShell to adjust the display formatting using `-AutoSize`.
- You can define a search scope by passing `-SearchScope`.
```
Get-ADObject -Filter * -SearchBase (Get-ADRootDSE) configurationNamingContext -SearchScope OneLevel
```
- You can search for a particular computer using `Get-ADForest -Server <server_name>`.
- You can also target accounts on a different computer:
```
Get-ADUser administrator -Properties * -Server <server_name>:3268`
```
- You can use a LDAP version using the parameter `-LDAPFilter`.
- You can request the credentials for an accont using `Get-Credential <domain>\<user>`.
- You can't do wildcards at the beginning and the end in LDAP: `*turtle*` is not allowed.

# Forensic Investigations and Lockouts
- 


# Stale Object Cleanup

# Managing and Troubleshooting AD Replication

# Recovery

# Deploying Domain Controllers

# Research:
- Implicit Remoting?


# Reference:
- [Microsoft Virtual Academy: Using PowerShell for Active Directory](https://aka.ms/MVAPSAD)
-
