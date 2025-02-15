# WSL(Windows Subsystem for Linux)

## Drive
`regedit:HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Lxss`
> `DefaultBasePath`: `D:\WSL\`


## Install
```powershell
wsl --list --verbose
```

`https://aka.ms/wslubuntu`
```powershell
curl.exe -L -o ubuntu.appx https://aka.ms/wslubuntu2204
Rename-Item .\ubuntu.appx Ubuntu.zip
Expand-Archive .\Ubuntu.zip -DestinationPath D:\WSL\Ubuntu
```
