# WSL(Windows Subsystem for Linux)

```powershell
wsl --list --verbose
```

`https://aka.ms/wslubuntu`
```powershell
curl.exe -L -o ubuntu.appx https://aka.ms/wslubuntu2204
Rename-Item .\ubuntu.appx Ubuntu.zip
Expand-Archive .\Ubuntu.zip -DestinationPath D:\WSL\Ubuntu
```
