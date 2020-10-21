Anti_debugging_protection_in_windows

## PEB : large fs:30h

https://en.wikipedia.org/wiki/Process_Environment_Block

Give metainformations on the program. So it will check if we are debugging.

---

## RDTSC

Bring timestamp two times. 
If we put a breakpoint in it, when it will compare, you will be detected.
Put a breakpoint after the instructions to bypass this one. (type F9 in IDA)


