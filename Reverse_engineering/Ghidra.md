# Ghidra

- Functions which are with _ before are created at the compilation.
- To patch a binary : right click on asm instruction, `patch instruction`
- Go to an address : `Navigation / Go To`
- ghidra cheat sheet : https://ghidra-sre.org/CheatSheet.html


## Practical example

- In the decompile entry, we have FindResourceA and LoadStringA.
- Searching in the Microsoft Doc, we know that it will store the value in a variable, and it will receive an identifier written in hex here.
- Windows / Defined Strings
- Going to this, we see lot of strings, if we double click on an address, it will go to the address in the memory. We see and ID in comments, so we just have to go to this ID to get what you want (pay attention, in decimal here).
