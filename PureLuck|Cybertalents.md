## [Pure luck](https://cybertalents.com/challenges/malware/pure-luck)

It’s an ELF 32-bit.
```console
m49di@DESKTOP-QB5N34E:~/CTF$ file pure-luck.out
pure-luck.out: ELF 32-bit LSB executable, Intel 80386, version 1 (GNU/Linux), statically linked, 
for GNU/Linux 2.6.32,BuildID[sha1]=939303fa92c4eb795fb6bf4e0941a81ba2b46436, not stripped
```
So first I tried to run strings command and grep for the flag but I didn't find any luck, but I found some text that gave
a hint that this file could be UPX packed.
```console
m49di@DESKTOP-QB5N34E:~$ strings pure-luck.out | cat -n | grep -i upx
     1  UPX!,
  4469  $Info: This file is packed with the UPX executable packer http://upx.sf.net $
  4470  $Id: UPX 3.92 Copyright (C) 1996-2016 the UPX Team. All Rights Reserved. $
  4477  UPX!u
  4974  UPX!
  4975  UPX!
```
So I unpacked it with the UPX unpacker and tried to run strings again but didn’t find the flag.
```console
m49di@DESKTOP-QB5N34E:~$ upx -d pure-luck.out
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2018
UPX 3.95        Markus Oberhumer, Laszlo Molnar & John Reiser   Aug 26th 2018

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
    737680 <-    300420   40.72%   linux/i386    pure-luck.out

Unpacked 1 file.
```
Then I went to ghidra to look at the assembly code and found something interesting .. that there are some values 
are being pushed to the stack and they aren't used.
```assembly
        080488f7 c6 45 dc 66     MOV        byte ptr [EBP + -0x24], 0x66
        080488fb c6 45 dd 6c     MOV        byte ptr [EBP + -0x23], 0x6c
        080488ff c6 45 de 61     MOV        byte ptr [EBP + -0x22], 0x61
        08048903 c6 45 df 67     MOV        byte ptr [EBP + -0x21], 0x67
        08048907 c6 45 e0 7b     MOV        byte ptr [EBP + -0x20], 0x7b
        0804890b c6 45 e1 55     MOV        byte ptr [EBP + -0x1f], 0x55
        0804890f c6 45 e2 50     MOV        byte ptr [EBP + -0x1e], 0x50
        08048913 c6 45 e3 58     MOV        byte ptr [EBP + -0x1d], 0x58
        08048917 c6 45 e4 5f     MOV        byte ptr [EBP + -0x1c], 0x5f
        0804891b c6 45 e5 69     MOV        byte ptr [EBP + -0x1b], 0x69
        0804891f c6 45 e6 73     MOV        byte ptr [EBP + -0x1a], 0x73
        08048923 c6 45 e7 5f     MOV        byte ptr [EBP + -0x19], 0x5f
        08048927 c6 45 e8 73     MOV        byte ptr [EBP + -0x18], 0x73
        0804892b c6 45 e9 6f     MOV        byte ptr [EBP + -0x17], 0x6f
        0804892f c6 45 ea 5f     MOV        byte ptr [EBP + -0x16], 0x5f
        08048933 c6 45 eb 65     MOV        byte ptr [EBP + -0x15], 0x65
        08048937 c6 45 ec 61     MOV        byte ptr [EBP + -0x14], 0x61
        0804893b c6 45 ed 61     MOV        byte ptr [EBP + -0x13], 0x61
        0804893f c6 45 ee 61     MOV        byte ptr [EBP + -0x12], 0x61
        08048943 c6 45 ef 61     MOV        byte ptr [EBP + -0x11], 0x61
        08048947 c6 45 f0 73     MOV        byte ptr [EBP + -0x10], 0x73
        0804894b c6 45 f1 79     MOV        byte ptr [EBP + -0xf], 0x79
        0804894f c6 45 f2 79     MOV        byte ptr [EBP + -0xe], 0x79
        08048953 c6 45 f3 7d     MOV        byte ptr [EBP + -0xd], 0x7d
```
Then I converted them to a characters using bash
```console
m49di@DESKTOP-QB5N34E:~$ echo "        080488f7 c6 45 dc 66     MOV        byte ptr [EBP + -0x24], 0x66
        080488fb c6 45 dd 6c     MOV        byte ptr [EBP + -0x23], 0x6c
        080488ff c6 45 de 61     MOV        byte ptr [EBP + -0x22], 0x61
        08048903 c6 45 df 67     MOV        byte ptr [EBP + -0x21], 0x67
        08048907 c6 45 e0 7b     MOV        byte ptr [EBP + -0x20], 0x7b
        0804890b c6 45 e1 55     MOV        byte ptr [EBP + -0x1f], 0x55
        0804890f c6 45 e2 50     MOV        byte ptr [EBP + -0x1e], 0x50
        08048913 c6 45 e3 58     MOV        byte ptr [EBP + -0x1d], 0x58
        08048917 c6 45 e4 5f     MOV        byte ptr [EBP + -0x1c], 0x5f
        0804891b c6 45 e5 69     MOV        byte ptr [EBP + -0x1b], 0x69
        0804891f c6 45 e6 73     MOV        byte ptr [EBP + -0x1a], 0x73
        08048923 c6 45 e7 5f     MOV        byte ptr [EBP + -0x19], 0x5f
        08048927 c6 45 e8 73     MOV        byte ptr [EBP + -0x18], 0x73
        0804892b c6 45 e9 6f     MOV        byte ptr [EBP + -0x17], 0x6f
        0804892f c6 45 ea 5f     MOV        byte ptr [EBP + -0x16], 0x5f
        08048933 c6 45 eb 65     MOV        byte ptr [EBP + -0x15], 0x65
        08048937 c6 45 ec 61     MOV        byte ptr [EBP + -0x14], 0x61
        0804893b c6 45 ed 61     MOV        byte ptr [EBP + -0x13], 0x61
        0804893f c6 45 ee 61     MOV        byte ptr [EBP + -0x12], 0x61
        08048943 c6 45 ef 61     MOV        byte ptr [EBP + -0x11], 0x61
        08048947 c6 45 f0 73     MOV        byte ptr [EBP + -0x10], 0x73
        0804894b c6 45 f1 79     MOV        byte ptr [EBP + -0xf], 0x79
        0804894f c6 45 f2 79     MOV        byte ptr [EBP + -0xe], 0x79
        08048953 c6 45 f3 7d     MOV        byte ptr [EBP + -0xd], 0x7d" | awk 'NF>1{print $NF}' | xxd -r -p
flag{UPX_is_so_eaaaasyy}
```
#### FLAG : flag{UPX_is_so_eaaaasyy}
