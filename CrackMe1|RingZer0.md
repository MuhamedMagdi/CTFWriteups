## [Crack Me 1](https://ringzer0ctf.com/challenges/9)
It's a PE32 executable
```console
m49di@DESKTOP-QB5N34E:~/CTF$ file CrackMe1.exe
CrackMe1.exe: PE32 executable (console) Intel 80386 (stripped to external PDB), for MS Windows
```
I tried to use ghidra but it didn't get me anywhere, so I decided to use PEView to perform some static analysis, then I found some indecators that this resource could be an executable file [dll or exe]
![resource](https://raw.githubusercontent.com/MuhamedMagdi/CTFWriteups/master/.assets/ctf.png?token=ALQU7EN7EQAUIIQU5WA7ZVK6T62XC)
so lets extract this resource with Resource Hacker and lets check if it's a dll or exe file with PEView.
![dll file](https://raw.githubusercontent.com/MuhamedMagdi/CTFWriteups/master/.assets/dll.png?token=ALQU7EK6MDGC2XANG52UPXC6T63IQ)
that have an exported function ``DisplayMessage`` 
![exported](https://raw.githubusercontent.com/MuhamedMagdi/CTFWriteups/master/.assets/exported.png?token=ALQU7EORY5PJC5UE3QTSII26T63SE)
which if we run the dll we would get the flag.
```console
C:\>rundll32 output.dll, DisplayMessage
```
![flag](https://github.com/MuhamedMagdi/CTFWriteups/blob/master/.assets/flag.png?raw=true)
#### FLAG : FLAG-hqoj2a5xkey9h6rmf44dc612v7
