SSP_(canari)

## How it works 

Add a secret value to the stack, called **canari**, just befeore the address in EBP.

If we write something with a buffer overflow after EBP, this value will be rewritten, and the program will be stopped.

Use checksec to verify : 
http://www.trapkit.de/tools/checksec.sh
`./checksec.sh --file canari`






