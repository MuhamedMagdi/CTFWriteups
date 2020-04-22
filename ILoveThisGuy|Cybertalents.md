## [I love this guy](https://cybertalents.com/challenges/malware/i-love-this-guy)
It’s a PE32 executable file.
```console
m49di@DESKTOP-QB5N34E:~/CTF$ file ScrambledEgg.exe
ScrambledEgg.exe: PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows
```
So lets run it and see what it does, it just ask you for the password and it will give you the flag so lets decompile it with ILSpy, we should foucse on the ``Button_Click`` function.
```csharp
    string value = new string(new char[5]
    {
        Letters[5],
        Letters[14],
        Letters[13],
        Letters[25],
        Letters[24]
    });
    if (TextBox1.Text.Equals(value))
    {
        ...
    }
```
If we figured out what is the value of ``value`` we would get the passowrd, and we know values of ``Letters`` array 
```csharp
    public char[] Letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ{}_".ToCharArray();
```
Lets get the password using python
```python
>>> Letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ{}_"
>>> Letters[5]+Letters[14]+Letters[13]+Letters[25]+Letters[24]
'FONZY'
```
password : FONZY
#### FLAG : FLAG{I_LOVE_FONZY}
