## What is it

- Group Policy Preferences allowed admins to create policies using embbedded credentials
- These credentials were encrypted and placed in a cPassword (xml file)
- The key was accidentally released !
- Patched in MS14-025 but doesn't prevent previous issues. If an admin has stored a group policy preferences embedded credentials before the patch, these credentials are exposed to us.


Reference : https://blog.rapid7.com/2016/07/27/pentesting-in-the-real-world-group-policy-pwnage/

---

## How to do it 

- In groups.xml, we find cpassword, and the name associated so we use gpp-decrypt :

`gpp-decrypt <cpassword>`

- Now we can connect to it with the user and password.

- If it doesn't work, maybe it's a service account with a tgs. So use GetUserSPNs.py to try it.



