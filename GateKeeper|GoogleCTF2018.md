## [GateKeeper](https://drive.google.com/file/d/1zlqcYJgVP3k3otj7PKmD7hKtB3yiIG9x/view)
It’s an ELF64-bit.
```console
m49di@DESKTOP-QB5N34E:~/CTF$ file gatekeeper
gatekeeper: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter 
/lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=a89e770cbffa17111e4fddb346215ca04e794af2, not stripped
```
When i run it, it asked me for a username and password.
```console
m49di@DESKTOP-QB5N34E:~/CTF$ ./gatekeeper
/===========================================================================\
|               Gatekeeper - Access your PC from everywhere!                |
+===========================================================================+
[ERROR] Login information missing
Usage: ./gatekeeper <username> <password>
```
Our job is to find the correct username and password to get the flag, so lets try using ltrace and search for ``strcmp(,)`` function
```console
m49di@DESKTOP-QB5N34E:~/CTF$ ltrace -o username ./gatekeeper magdi 1234
/===========================================================================\
|               Gatekeeper - Access your PC from everywhere!                |
+===========================================================================+
 ~> Verifying....
ACCESS DENIED
 ~> Incorrect username
m49di@DESKTOP-QB5N34E:~/CTF$ cat -n username | grep strcmp
   892  strcmp("magdi", "0n3_W4rM")                                               = 61
```
It tried to compare what I gave it as a username with ``0n3_W4rM`` so I think we found the correct username, lets try to find the password with the same method after providing the correct username.
```console
m49di@DESKTOP-QB5N34E:~/CTF$ ltrace -o password ./gatekeeper 0n3_W4rM 1234
/===========================================================================\
|               Gatekeeper - Access your PC from everywhere!                |
+===========================================================================+
 ~> Verifying.......ACCESS DENIED
 ~> Incorrect password
m49di@DESKTOP-QB5N34E:~/CTF$ cat -n password | grep strcmp
   892  strcmp("0n3_W4rM", "0n3_W4rM")                                            = 0
  1050  strcmp("4321", "zLl1ks_d4m_T0g_I")                                        = -70
```
It reversed my password and compared it with "zLl1ks_d4m_T0g_I" so the correct password must be ``I_g0T_m4d_sk1lLz``
```console
m49di@DESKTOP-QB5N34E:~/CTF$ ./gatekeeper 0n3_W4rM I_g0T_m4d_sk1lLz
/===========================================================================\
|               Gatekeeper - Access your PC from everywhere!                |
+===========================================================================+
 ~> Verifying.......Correct!
Welcome back!
CTF{I_g0T_m4d_sk1lLz}
```

#### FLAG : CTF{I_g0T_m4d_sk1lLz}
