## [Getting started](https://cybertalents.com/challenges/malware/getting-started)
It's an ELF 32-bit file.
```console
m49di@DESKTOP-QB5N34E:~/CTF$ file getting-started
getting-started: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter
/lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=30a1e294daa93e231641dbe11a9501f87ed4a1c3, not stripped
```
So first I tried to run strings on the file and found something interesting .. it could be the flag.
```console
m49di@DESKTOP-QB5N34E:~/CTF$ strings getting-started | cat -n | grep {
     8  j}j1j_jljejvjejlj_jojtj_jejmjojcjljejwj{jgjajljf
```
Then I tried to remove all the ‘j’ with python and reverse it.
```python
>>> flag = 'j}j1j_jljejvjejlj_jojtj_jejmjojcjljejwj{jgjajljf'
>>> out = ''
>>> for c in flag:
...     if c != 'j':
...             out += c
...
>>> out
'}1_level_ot_emoclew{galf'
>>> out [::-1]
'flag{welcome_to_level_1}'
```
Lets try another approach with ghidra if you opened the file with ghidra you will find some interesting values are being pushed into the stack.
```assembly
        080483e8 6a 7d           PUSH       0x7d
        080483ea 6a 31           PUSH       0x31
        080483ec 6a 5f           PUSH       0x5f
        080483ee 6a 6c           PUSH       0x6c
        080483f0 6a 65           PUSH       0x65
        080483f2 6a 76           PUSH       0x76
        080483f4 6a 65           PUSH       0x65
        080483f6 6a 6c           PUSH       0x6c
        080483f8 6a 5f           PUSH       0x5f
        080483fa 6a 6f           PUSH       0x6f
        080483fc 6a 74           PUSH       0x74
        080483fe 6a 5f           PUSH       0x5f
        08048400 6a 65           PUSH       0x65
        08048402 6a 6d           PUSH       0x6d
        08048404 6a 6f           PUSH       0x6f
        08048406 6a 63           PUSH       0x63
        08048408 6a 6c           PUSH       0x6c
        0804840a 6a 65           PUSH       0x65
        0804840c 6a 77           PUSH       0x77
        0804840e 6a 7b           PUSH       0x7b
        08048410 6a 67           PUSH       0x67
        08048412 6a 61           PUSH       0x61
        08048414 6a 6c           PUSH       0x6c
        08048416 6a 66           PUSH       0x66
```
So lets convert them to char values and reverse the output.
```console
m49di@DESKTOP-QB5N34E:~/CTF$ echo "        080483e8 6a 7d           PUSH       0x7d
        080483ea 6a 31           PUSH       0x31
        080483ec 6a 5f           PUSH       0x5f
        080483ee 6a 6c           PUSH       0x6c
        080483f0 6a 65           PUSH       0x65
        080483f2 6a 76           PUSH       0x76
        080483f4 6a 65           PUSH       0x65
        080483f6 6a 6c           PUSH       0x6c
        080483f8 6a 5f           PUSH       0x5f
        080483fa 6a 6f           PUSH       0x6f
        080483fc 6a 74           PUSH       0x74
        080483fe 6a 5f           PUSH       0x5f
        08048400 6a 65           PUSH       0x65
        08048402 6a 6d           PUSH       0x6d
        08048404 6a 6f           PUSH       0x6f
        08048406 6a 63           PUSH       0x63
        08048408 6a 6c           PUSH       0x6c
        0804840a 6a 65           PUSH       0x65
        0804840c 6a 77           PUSH       0x77
        0804840e 6a 7b           PUSH       0x7b
        08048410 6a 67           PUSH       0x67
        08048412 6a 61           PUSH       0x61
        08048414 6a 6c           PUSH       0x6c
        08048416 6a 66           PUSH       0x66" | awk 'NF>1{print $NF}' | xxd -r -p | rev
flag{welcome_to_level_1}
```
FLAG : flag{welcome_to_level_1}
