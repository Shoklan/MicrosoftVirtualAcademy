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
- 


# Quering AD Data

# Forensic Investigations and Lockouts

# Stale Object Cleanup

# Managing and Troubleshooting AD Replication

# Recovery

# Deploying Domain Controllers

# Research:
- Implicit Remoting?


# Reference:
- [Microsoft Virtual Academy: Using PowerShell for Active Directory](https://aka.ms/MVAPSAD)
-
