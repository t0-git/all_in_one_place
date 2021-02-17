## Tips and tricks 

### Easy 

- continue one instruction or one function after : `nexti`
- continue one instruction after or inside another function : `stepi`
- see 8 instructions after the beginning of rip : `x/8i $rip`
- breakpoint : `b*(address)`
- delete breakpoint 1 : `delete 1`
- view breakpoint : `info breakpoint`
- View values in registry : `x/8wx $rax` => view 8 words in hex after rax (you can print byte (`b`), `h` (half word), `g` (giant use it in 64 bits))
- print address : `/x $register`
- print functions : `info functions`


## Useful

- Change value (for example to a nop : 0x90) : `set *(unsigned char*)address = 0x90`. **Do not forget to change value of all the address between the current address and the next one, so if there's 4 bytes between the current instruction and the next one, add 4 nops.**

- find the address of a function during execution : `b*main`, `run`, `info address function`
- you want to go into this ? `set ($rip)=address`

- launch the program with an external argument : `r $(cat test.txt)`

## Generate patterns to find where we can write (buffer overflow)

```
pattern_create 100
execute the program
pattern_search
```
or 
`python -c 'print("A"*170)'`

## Bypass ptrace protection
```
catch syscall ptrace
commands 1
set ($eax) = 0
continue 
end
```

Another way to do this : 
```
#include <unistd.h>
#include <stdio h>
#include <linux/ptrace .h>

long ptrace(int request, int pid, void *addr, void *data) {
	printf("ptrace has been invoked ! \n");
	if ( request == PTRACE_TRACEME )
	printf("This process is trying to ptrace itself \n");
	/* else {
		--asm-- --volatile-- (
		"movl 20(%ebp), %esi\n"
		"movl 16(%ebp), %edx\n"
		"movl 12(%ebp), %ecx\n"
		"movl 8(%ebp), %ebx\n"
		"movl $0x1A, %eax\n"
		"int $0x80\n"
		); } */
	return 0;
}

gcc -Wall -c antiantiptrace.c -o antiantiptrace.o
gcc -shared -o antiantiptrace.so antiantiptrace.o
gdb binary
set environment LD_PRELOAD ./antiantiptrace.so
```
