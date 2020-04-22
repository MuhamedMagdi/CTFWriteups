## [Can you see through the star](https://ringzer0ctf.com/challenges/110)
It’s a PE32 executable.
```console
m49di@DESKTOP-QB5N34E:~/CTF$ file Canyouseethroughthestar.exe
Canyouseethroughthestar.exe: PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows
```
It just gives you one button when you click on it, it showes the flag but in password format, so lets jumb into ILSpy and see the decompiled version. You should foucse on the ``button1_Click`` function. It using ``Concat`` function to generete the flag.
```csharp
	private void InitializeComponent()
	{
    ...
    button1.Name = "button1";
    ...
		maskedTextBox1.Name = "maskedTextBox1";
    ...
	}

	private void button1_Click(object sender, EventArgs e)
	{
		maskedTextBox1.Text = string.Concat(string.Concat("FLAG-" + maskedTextBox1.Name, "vc"), button1.Name);
	}
  ```
  ### FLAG : FLAG-maskedTextBox1vcbutton1
