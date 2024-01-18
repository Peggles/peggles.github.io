# peggles.github.io

## PowerShell profile
```powershell
# Default parameter values.
$PSDefaultParameterValues['Start-WhatToDo:ConfigurationPath'] = "$($env:USERPROFILE)\Documents\Filer\Dev\PowerShell\WhatToDo\WhatToDo-Config.psd1"

# Aliases.
Set-Alias -Name 'ConnectServer' -Value 'Connect-PSProfileServer'
Set-Alias -Name 'ConnectSwitch' -Value 'Connect-PSProfileSwitch'

function Prompt {
    $Host.UI.RawUI.WindowTitle = "PowerShell $((Get-Host).Version)"

    $identity = [Security.Principal.WindowsIdentity]::GetCurrent()
    $principal = [Security.Principal.WindowsPrincipal] $identity
    $adminRole = [Security.Principal.WindowsBuiltInRole]::Administrator

    if (Test-Path -Path variable:/PSDebugContext) {
        $Host.UI.RawUI.WindowTitle = "[DEBUG] $($Host.UI.RawUI.WindowTitle)"
        Write-Host "[DEBUG] " -NoNewline -ForegroundColor Magenta
    }
    elseif ($principal.IsInRole($adminRole)) {
        $Host.UI.RawUI.WindowTitle = "[ADMIN] $($Host.UI.RawUI.WindowTitle)"
        Write-Host "[ADMIN] " -NoNewline -ForegroundColor Red
    }

    if ($PWD.Provider.Name -ne 'FileSystem') {
        Write-Host "[$($pwd.Provider.Name)]" -NoNewline -ForegroundColor DarkGray
    }

    # Print parts of the path in different colors.
    $pathList = $PWD.Path.Split('\')
    for ($i = 0; $i -lt ($pathList | Measure-Object).Count; $i++) {
        if ($i -lt ($pathList | Measure-Object).Count - 1) {
            # All path items except final.
            Write-Host $pathList[$i] -NoNewline -ForegroundColor DarkBlue
            Write-Host '\' -NoNewline -ForegroundColor DarkGray
        }
        else {
            # Final path item.
            Write-Host $pathList[$i] -NoNewline -ForegroundColor Blue
        }
    }

    Write-Host '>' -NoNewline -ForegroundColor DarkGray
    return ' '
}

# Source all files in a custom profile functions folder. One function per ps1 file (with same filename as the function).
Get-ChildItem -Path (Join-Path -Path $PSScriptRoot -ChildPath 'ProfileFunctions') -Filter '*.ps1' | ForEach-Object { . $_.FullName }
```

## Windows Terminal
Settings file for Windows 10/11: %LOCALAPPDATA%\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json
