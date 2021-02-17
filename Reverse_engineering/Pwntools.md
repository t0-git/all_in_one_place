## Tips

- terminal.context : `("termite", "-e")`
- write asm code : `asm('sub edx, 0x1f; jmp edx')`
- write little endian address in 32 bits : `jmp_esp = p32(0x804919f)`
- write in bytes : `a = b"A" * 4`

- launch in local : 
```
vuln = gdb.debug('/home/toto/Downloads/space', '''
b*vuln
continue
''')
vuln.recvuntil(">")
vuln.sendline(payload)
vuln.interactive()
```