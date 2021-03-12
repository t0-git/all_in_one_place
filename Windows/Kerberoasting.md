## Explanation :

Key Distribution Center : KDC
KDC holds all the keys.

- User is going to authenticate to request a Ticket Granting Ticket with an NTLM hash.
- It will receive it encrypted with kerberos granting ticket hash.
- Next we need to access to a service. This service has a SPN (Service Principal Name).
- To access to this service, we need a Ticket Granting Service (TGS).
- The user presents its TGT to request a TGS to KDC.
- The KDC gives the TGS encrypted with server's account hash to the user.
- Then the user presents his TGS to the application server decrypted with his own server hash and say yes you're authorized to authenticate or not.


## How to

This module will try to find Service Principal Names that are associated with normal user account. Since normal account's password tend to be shorter than machine accounts, and knowing that a TGS request will encrypt the ticket with the account the SPN is running under, this could be used for an offline bruteforcing attack of the SPNs account NTLM hash if we can gather valid TGS for those SPNs. This is part of the kerberoast attack researched by Tim Medin (@timmedin)

`getuserspns.py <domain>/<user>:<password> -dc-ip <dc-ip> -request`

- save the hash (everything) and try to crack it with hashcat


## Mitigation

- Strong passwords
- Least privilege

