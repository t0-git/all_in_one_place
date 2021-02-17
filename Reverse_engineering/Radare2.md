## Tips : 
- Open the binary with `-d` to not have any pb with entrypoints or else
- Open the binary with `-w` to write what you want

## How to debug quickly : 

- Analyze code : `aaa`
- print functions : `afl`
- decompile function : `pdf @ <function>` (print disassemble function)
- When you disassemble a function look at the beginning, you have the address of the variable printed
- show the value of the variable : `px @ rbp-0x4`
- Put breakpoint : `db <address>`
- continue : `dc` 
- view registers : `x @ <register>`
- if you want to trace all the call, after `aaa`, launch `dcs`

- change the layout : `e asm.syntax=att` or `e asm.syntax=intel`


- `ood` to launch in debug mode
- `oo+` to open the file in debug and write mode

- `s main` : go to main function
- `pdf` : print it

- `dr` : print registry values

- problem to pass an argument (for example unsigned int : -998732) :
1. Write it in a text file (file.txt)
2. then `r2 -d <binary>`
3. 
```
ood `(!cat file.txt)`
```

## Modify a value

- Write mode :
- `r2 -w binary`
- `aaa`
- `wa <instruction> @address`
- exit and execute the program



## Analyze the file

- `i` : information
- `i?` => what we can do
- `aaa` : analyze file
- `afl` : print functions
- `ie` : find entrypoints

- `f` : print everything

- `fs` : organize f
- `fs functions`

## Lost because the program launch and nothing ?

- `iz` to print strings
- find the Vaddr
- find the references : `axt <Vaddr>`
- `s <value printed>`
- print 50 lines : `pd 50`
- then find in which function you are, going to previous values (for example, you are in 0x11ea => s 0x1100)
- jump to the address you want to go to : `s <address>`, `oo+` 
- `wa <instruction> @<address>`

## Bypass ptrace debugging detection 

- verify if it calls it : `dcs ptrace`
- launch the binary as usual
- then:
```
(hooker, dr rax=0, dc);db $$+5 @@=`axt sym.imp.ptrace~CALL~call[1]`;dbc $$+5 .(hooker) @@=`axt sym.imp.ptrace~CALL~call[1]
```
