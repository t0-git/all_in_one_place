## PowerSploit

In cmd in the victim machine, when you are logged in, download powersploit (git), then paste the folder onto it.

- in powershell : echo `$Env:PSModulePath`
- Put the folder into the previous path
- then : `powershell -ep bypass`
- `Import-Module PowerSploit`


### Get-NetDomain
=> name of the domain

### Get-NetDomainController
=> Information about the domain controller

### Get-DomainPolicy
=> information about the policy

### if you want detail, for example here about system access in domain policy :
`(Get-DomainPolicy)."system access"`

### Get-NetUser
=> Information about users

### Filtering 
`Get-NetUser | select <something>`
for example cn, description, samaccountname ...
	
## Get-UserProperty

then select :
	
### Get-UserProperty -Properties accountexpires 
or something else (badpwdcount...)

### Get-NetComputer -FullData | select (something)

### Get-NetGroup -GroupName "Domain Admins"

### Get-NetGroup -GroupName *admin*

### Get-NetGroupMember -GroupName "Domain Admins"

### Invoke-ShareFinder
=> View all shares available

### View GPO
=> `Get-NetGPO`

https://github.com/PowerShellMafia/PowerSploit