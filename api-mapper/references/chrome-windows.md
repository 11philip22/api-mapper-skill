# Chrome Executable Path — Windows

## Common install locations (try in order)

```
C:\Program Files\Google\Chrome\Application\chrome.exe
C:\Program Files (x86)\Google\Chrome\Application\chrome.exe
%LOCALAPPDATA%\Google\Chrome\Application\chrome.exe
```

`%LOCALAPPDATA%` expands to `C:\Users\{username}\AppData\Local\`.

## Auto-detect via PowerShell

```powershell
$paths = @(
  "C:\Program Files\Google\Chrome\Application\chrome.exe",
  "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe",
  "$env:LOCALAPPDATA\Google\Chrome\Application\chrome.exe"
)
$chrome = $paths | Where-Object { Test-Path $_ } | Select-Object -First 1
Write-Host "Chrome: $chrome"
```
