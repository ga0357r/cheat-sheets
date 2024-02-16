# PowerShell Cheat-Sheet
PowerShell is a task automation and configuration management program from Microsoft, consisting of a command-line shell and the associated scripting language.

## Install PowerShell
PowerShell was made open-source and cross-platform with PowerShell Core, and can be installed on multiple operating systems.

### Windows
1. Download MSI Package from the [Official PowerShell Docs](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.2)
2. Set up PowerShell Profile in [Windows Terminal](windows/windows-terminal.md).
```json
"commandline": "pwsh.exe -nologo",
"name": "Powershell",
"source": "Windows.Terminal.PowershellCore"
```

### Code Sheet
#### Start a Background Process
```
Start-Process "<processName>" -ArgumentList "<argumentName>" -WindowStyle Hidden
# For example : Start minikube dashboard in background
Start-Process "minikube" -ArgumentList "dashboard" -WindowStyle Hidden
```
