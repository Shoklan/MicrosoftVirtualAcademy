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
- The **Metadata** is what keeps track of who and when changes occurred.
- You can get that data using `Get-ADReplicationAttributeMetaData`.
- The **RID** of the Enterprise Admin Account is *-519*.
- USN stands for **Update Sequence Number**.
- The **Invocation Id** is unique per database.
- There is an alias for the `Out-GridView` of `ogv`.
- To show group memberships, since 2003, you need to pass the parameter `-ShowAllLinkedValues`.
- The Attributes **FirstOriginatingCreateTime** will show you when a membership was added to a user.
- There is the `Get-EventLog` which looks at the classic Windows event logs.
- The event logs are in XML when you use `Get-WinEvent`; which supersedes `Get-EventLog`.
- You can get the XML from *Event Viewer > Filter Current Log > XML Tabular*.
- Take that and insert it into:
```
[xml]$xmlfilter = @"
<xml_here>
"@
```
- A **Here-String** is Powershells version of a string literal.
- Please note that the `"@"` must be left justified or it wont work: maybe.
- You can run a command on a remote server using `Invoke-Command`.
- You can check which accounts are **Locked Out** using the parameter `-LockedOut`.
- You can exclude properties using `ExcludeProperty`.
- You can get the ACLS of an object using `Get-Acl`.

# Stale Object Cleanup
- Make you have the Recycling Bin on in case you need to undo changes.
- You can use `Get-Date` in a multitude of ways:
  * `[datetime]::Now`
  * `[datetime]::UtcNow`
  * `[datetime] | gm -Static`; means get static members.
  * `(Get-Date).ToFileTimeUTC()`
  * `(Get-Date).ToFileTime()`
- If you give `FromFileTime()` the digit representation of the number, then you get a formatted date back.
- You can get a list of inactive users using `Search-ADAccount -AccountInactive -UserOnly | ogv`.
- You can generate your own time span using `New-TimeSpan -Days 90`.
- You can tell powershell to check for inactive computers using `-ComputersOnly` instead.
- You can check for Operating Systems using `$xp = Get-ADComputer -Filter {OperatingSystem - like "*XP*"}`.
- **kbrtgt** is for Kerberos to create tickets.
- **Resume Producing Event** or RPE.


# Managing and Troubleshooting AD Replication
- **Flexible Single Master Operation** is what fsmo stands for.
- You can discovery who the *Primary Domain Controller* is using `Get-ADDomainController -Service PrimaryDC -Discover`.
- You can set the roles using `Move-ADDirectoryServerOperationMasterRole -Identity <pcname> - OperationMasterRrole PDCEmulator, SchemaMaster`.
- You can move multiple roles at the same time.
- You can create new sites from csv using `Import-Csv <file> | New-ADReplicationSite`
- You can create new subnets from csv using `Import-Csv <file> | New-ADReplicationSubnet`
- You can see what is queued up for replication using `Get-ADReplicationQueueOperation -Server <cmp_name>`.
- You can stop a computer using `Stop-Computer -ComputerName <cmp_name> -Force`.
- You can check on Replication errors using `Get-ADReplicationFailure -Scope <Forest | Doamin | Server | Site>`.

# Recovery
- 


# Deploying Domain Controllers

# Research:
- Implicit Remoting?


# Reference:
- [Microsoft Virtual Academy: Using PowerShell for Active Directory](https://aka.ms/MVAPSAD)
- [Active Directory Topology Diagrammer](https://www.microsoft.com/en-us/download/details.aspx?id=13380)
- [Everything You Need to Work with Active Directory](http://www.aka.ms/AD101)
-
