Tips

## Padded kernel file

- a padded file : everything which is non system is erased by 0's, so you will not have access to the user land.

- But if you search something which you thought was in the user land, we have to search in the cache, in the ram, because the access time are better in the ram. So we can find informations in the ram.

## Find the profile of a dump

- Use ```strings``` on it to find relevant information

- ```vol.py --plugins=<path_to_profile> -f <file_decompressed> imageinfo```


## Compressed dump

- if you know that your dump was compressed, use zlib to uncompress it :
```import zlib
compressed = open('memory', 'rb').read()
decompressed = zlib.decompress(compresed)
f = open('decompressed', 'wb')
f.write(decompressed)
f.close
```

- Then use ```strings``` on it to find relevant information (grep "vmlinuz" for kernel in Linux, os, x64 etc ...)

---

## Options of a profile

- ```vol.py --plugins=\<path_to_profile\> --info```

Then use the good profile :

- ```vol.py --plugins=<path_to_profile> --profile=<profile> -f file```
- 
---

## ELF File

- Like a PE in windows.
- Have all the symbols, tables etc... to execute it without anything else.
- The debugging file of this is a DWARF file.
- So a kernel is a binary initialized, and will launch init. 
- If it's stripped, you will not have all the name of the functions etc ...
- ```objdump``` can open an elf file
- with -t il will print the symbols, symbols are instructions in assembly code

---

## When you have kernel version

- Download the debug version
- Extract it with tar
- We only need the binary of the kernel, not the bootloader etc.
- the file we need is vmlinux...
- ```wget -O extract-vmlinux https://...``` in torvalds repository is a script to extract it
- use the script to extract it to an elf file
- then use ```dwarf2json``` to have a json file to ingest in volatility
- move this json file in the symbol folder of volatility and change the format to the same as the others
- ```python3 vol.py -f dump <plugin>```

---

## Find file in linux with volatility

- linux_find_file -h

- list all files : -L

- dump a folder : -D 

- specify the inode you want to select : -i 

- outfile you want -O

---

## Lime

- check if the format is padded, or else (try to find the command executed)

---

## Check this out

- The Sleuth kit
