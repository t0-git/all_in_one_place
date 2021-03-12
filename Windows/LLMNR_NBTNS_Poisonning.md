## Theory

Used to identify hosts when DNS fails to do so.

Previously NBT-NS. (Netbios Name Service)

The service utilize a user's username and NTLMv2 hash when appropriately responded to.

---

## Exploit it

Use responder to extract the hash :

`responder -I <interface> -rdwv`

Then on the victim machine, try to connect to a smb share that doesn't exist and you'll see the hash.

---

## LLMNR Poisonning defense

- Disable LLMNR

Turn off multicast name resolution :

Local computer Policy > Computer Configuration > Administrative Templates > Network > DNS Client in the Group Policy Editor

- Disable NBT-NS

Network Connections > Network Adapter Properties > TCP/IPv4 Properties > Advanced Tab > WINS tab and select Disable NetBios over TCP/IP

### If it's not possible to disable it

- Enable Network Access Control : it's not possible in that way to go inside the company and plug in on ethernet port or other network (wireless) and do this type of attack
- To prevent cracking of hash passwords, use strong user passwords > 14 characters and limit common word usage. But this is not the good solution, as there is the pass the hash attack.
