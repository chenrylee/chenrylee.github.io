# Create PowerShell Session via SSH
Maybe you want to manage your Windows server from you Macbook or Linux server, OK, PowerShell Core supports it. Follow me to configure your Windows Server:  
## 1. Install PowerShell Core
Download PowerShell core from [PowerShell Core Releases](https://github.com/PowerShell/PowerShell/releases/latest), and extract.
## 2. Install and Configure SSH Server
### 2.1 Install
```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```
### 2.2 Configure
Edit the `sshd_config` file located at `$env:ProgramData\ssh`.
Make sure password authentication is enabled:
```bash
PasswordAuthentication yes
```
Create the SSH subsystem that hosts a PowerShell process on the remote computer:
```bash
Subsystem powershell c:/progra~1/pwsh/pwsh.exe -sshs -NoLogo -NoProfile
```
To get the short path, use this cmdlet:
```powershell
Get-CimInstance Win32_Directory -Filter 'Name="C:\\Program Files"' | Select-Object EightDotThreeFileName
```
### 2.3 Start the service
Set the startup type to `automatic`, in case your server is rebooted.
```powershell
Set-Service -Name ssh-agent -StartupType 'Automatic'
Set-Service -Name sshd -StartupType 'Automatic'
Start-Service ssh-agent
Start-Service sshd
```
Make sure the firewall allows SSH inbound connections.

## Connect to Your Server
If you are running a Windows client, make sure PowerShell Core is your current shell.
```powershell
$s = New-PSSession -HostName 10.10.10.38 -UserName chenry -Port 22
Import-PSSession $s -Module ActiveDirectory
```

Enjoy!
