## RELRO

- The RELRO stands for ‘relocation read-only’ and this protection ensures that the global offset table (GOT) cannot be overwritten. But in this case, it’s partial RELRO so the only pragmatic difference is that the BSS section comes before the GOT. This prevents buffer overflows in global variables overwriting the GOT.

---

## Stack canary

- The stack options refer to stack canaries. This is a value called the stack canary that is placed on the stack. If the stack canary is overridden during a buffer overflow execution of the binary will terminate with a stack smashing error. In this case, the stack canary is not enabled making exploitation much easier.

---

## NX

- Next, the NX flag, if enabled at compile-time, indicates that a given memory region can be either readable or executable but never both. Again this is a protection that makes it harder to execute arbitrary shellcode.

---

## PIE

- The PIE option stands for position independent executable. A binary without this protection uses virtual addresses that are static. On the other hand, if PIE is enabled it means that Adress Layout Randomization (ASLR) can be used to randomize where the binary is stored in memory if this feature is enabled in the kernel. This makes reliably finding return address much harder. 