## Command execution

- pay attention if it's python2 or python3
- Find the character to escape 
- ensure it's a python application (good chance : `%2Bstr(True)%2B`)
- inside str, if you put `os.system('ls')` and 0 on return, you have it. (command executed successfuly). To have the value returned, use instead `os.popen('<cmd>').read()`
- if os is not imported : `__import__('os').popen...`
- use base64 : `__import__('os').popen(__import_('base64').b64decode()))`

Try different escaping characters `" '` and concatenation `+ .`