# Windows 11 Bloatware Removal Script

A PowerShell script to silently uninstall pre-installed Microsoft apps from Windows 11.

## What It Removes

### MSI-Based Apps (via Registry Uninstall)
Removes language-specific installs across **en-us, es-es, fr-fr, and pt-br**:

- Microsoft OneNote
- Microsoft 365

## Requirements

- Windows 11
- PowerShell 5.1 or later
- **Administrator privileges** (required)

## Usage

### 1. Allow script execution (one-time, current session only)
```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

### 2. Run the script
Right-click PowerShell → **Run as Administrator**, then:
```powershell
.\Remove-Bloatware.ps1
```

## How It Works

The script searches both 32-bit and 64-bit registry uninstall paths for each app/language combination and silently invokes the uninstaller:

```
HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall
```

The `displaylevel=false` flag suppresses any installer UI during removal.

## Adding More Languages

Edit the `$LanguageList` array at the top of the script:

```powershell
$LanguageList = "en-us","es-es","fr-fr","pt-br","de-de","it-it"
```

## Adding More Apps

Edit the `$AppList` array:

```powershell
$AppList = "Microsoft OneNote","Microsoft 365","Microsoft Publisher"
```

> **Note:** App names must match the `DisplayName` value in the registry exactly.

## Before Running

The script as shared uses `cmd(.)exe` to prevent accidental execution. Before running, you must remove the parentheses around the period so it reads:

```powershell
Start-Process -WindowStyle Hidden cmd.exe -ArgumentList '/c', $uninst -Wait -PassThru
```

## Disclaimer

- Test in a virtual machine before deploying broadly
- Use at your own risk — removing system components may affect functionality
- Some apps may reinstall after Windows updates; re-running the script will remove them again

## Credits
Original script sourced from the Spiceworks Community:
https://community.spiceworks.com/t/removing-preinstalled-office-365-versions-with-powershell-or-batch-script/945257/17
