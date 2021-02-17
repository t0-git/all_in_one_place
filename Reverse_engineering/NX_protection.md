## Check if it's enabled

Know the flags of the stack :

***$ readelf -l add32 | grep GNU_STACK***

If there's only R and W, the stack is not executable, NX protection is enabled.

## Using libc to call system


find the base address of libc with ldd :

- `ldd <program to overflow> | grep libc`

then use readelf to get the offsets to various functions and grep system and exit :

- readelf -s <path find earlier> | grep <function needed>`

then use strings to get the strings from libc with hex offsets and grep /bin/sh

- `strings -a -t x <path find earlier> | grep /bin/sh`

next calculate the address for each
```
address of libc + address of system
address of libc + address of exit
address of libc + addres of /bin/sh
```
then `payload = <overflow> + >address of system> + <first ret address overwritten> + <address exit> + <next return address> + </bin/sh address>

why put exit address ? when it will arrive at the end of the system function, it will search for a return address, and to close it properly, put the address of exit, so it will execute properly, and then it will use the /bin/sh which is at the top of the stack.

---

## Complete explanation and comprehensive in french

https://beta.hackndo.com/retour-a-la-libc/

---
