## [Crack Me 1](https://ringzer0ctf.com/challenges/9)
It's a PE32 executable
```console
m49di@DESKTOP-QB5N34E:~/CTF$ file CrackMe1.exe
CrackMe1.exe: PE32 executable (console) Intel 80386 (stripped to external PDB), for MS Windows
```
I tried to use ghidra but it didn't get me anywhere, so I decided to use PEView to perform some static analysis, then I found some indicators that this resource could be an executable file [dll or exe]
![resource](https://github.com/MuhamedMagdi/CTFWriteups/blob/master/.assets/ctf.png?raw=true)
so let's extract this resource with Resource Hacker and lets check if it's a dll or exe file with PEView.
![dll file](https://github.com/MuhamedMagdi/CTFWriteups/blob/master/.assets/dll.png?raw=true)
that have an exported function ``DisplayMessage`` 
![exported](https://github.com/MuhamedMagdi/CTFWriteups/blob/master/.assets/exported.png?raw=true)
which if we run the dll we would get the flag.
```console
C:\>rundll32 output.dll, DisplayMessage
```
![flag](https://github.com/MuhamedMagdi/CTFWriteups/blob/master/.assets/flag.png?raw=true)
#### FLAG : FLAG-hqoj2a5xkey9h6rmf44dc612v7
