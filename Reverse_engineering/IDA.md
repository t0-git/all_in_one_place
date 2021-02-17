
## Increase number of bytes in instructions : 

- Options
- General
- Disassembly
- Number of opcodes bytes (non-graph) : 6 minimum

---

## Multiple View

- View
- Open SubView
- Disassembly

---

## View strings 

- View
- Open SubView
- Strings


## In graph view :


- double click on function to go into it
- same for instructions
- then when you are in code view, escape to return to graph view
- back : CTRL + Enter
- CTRL + Scroll to zoom in or out
- Rename a function : click one, then type N (rename it f_function, f to know that it's my renaming). Erase it to go back to the generic name

### Comments

- Type in INSERT to insert anterior comment ("; anterior comment")
- SHIFT + INSERT for posterior comment
- Type in : for colon comment (next to the function)
- Type in ; for repeate comment (repeated on each call of the function) 

---

## In text view :

- Select a value, and type B to convert value in binary
- If you want to view it in other formats, just right click on the value (don't select it before)
- Click on a function, then type X to see all the references of this function in the program
- Text search : ALT+T
- If there is a part where you see a string but one letter by line, type A on this part, and undefine with U. If you want to assemble one instruction with another, not a block, press D. If you undefine too much, press C.

---

## Saving data 

**Snapshot**

- View
- Database snapshot manager

**Save your file**

---

## Pseudo code

- select a function
- click on the green button READY
- view / graph views / generate graph views
- Pay attention, some values are in decimal, other in hexa !


---

## Debug in IDA

### How to

- select your debugger 
- run

### Change instructions in debug :

- type F8 to continue in the program
- when you stop on the needed instruction, two choices :
- Edit, patch program, assemble, and change the instruction
- or select the instruction you want to go, right click, set ip


---

## Modify instructions in debug

- put a breakpoint in graph view (right click, add breakpoint, edit breakpoint and select hardware)
- when you arrive to the jump, click on the instruction you want to go and then : SET IP
- or select the instruction, edit, patch program, assemble, then modify your code

