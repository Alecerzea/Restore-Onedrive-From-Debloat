# Restore-Onedrive-From-Debloat
Restore the access to onedrive after you debloated your system using either Talon or Win11Debloat

# Stop OneDrive and clear registry blocks
taskkill /f /im OneDrive.exe 2>$null
Remove-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\OneDrive" -Name "DisableFileSyncNGSC" -ErrorAction SilentlyContinue
Remove-ItemProperty -Path "HKLM:\SOFTWARE\Wow6432Node\Policies\Microsoft\Windows\OneDrive" -Name "DisableFileSyncNGSC" -ErrorAction SilentlyContinue
Set-ItemProperty -Path "HKCR:\CLSID\{018D5C66-4533-4307-9B53-224DE2ED1FE6}" -Name "System.IsPinnedToNameSpaceTree" -Value 1 -ErrorAction SilentlyContinue

# Fix folder ownership and visibility
takeown /f "$env:USERPROFILE\OneDrive" /r /d y
icacls "$env:USERPROFILE\OneDrive" /grant "${env:USERNAME}:F" /t /q
attrib -h -s "$env:USERPROFILE\OneDrive"

# Reset the OneDrive application
if (Test-Path "$env:LOCALAPPDATA\Microsoft\OneDrive\onedrive.exe") { 
    Start-Process "$env:LOCALAPPDATA\Microsoft\OneDrive\onedrive.exe" -ArgumentList "/reset" 
} elseif (Test-Path "$env:ProgramFiles\Microsoft OneDrive\onedrive.exe") { 
    Start-Process "$env:ProgramFiles\Microsoft OneDrive\onedrive.exe" -ArgumentList "/reset" 
}
