# WinRM

## Server

Enable the remoting.

[Enable-PSRemoting](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-7.3)

```powershell
Enable-PSRemoting
```

## Client

Add a host to the trusted list for NTLM connection:

```powershell
Set-Item WSMan:\localhost\Client\TrustedHosts example.local -Concatenate -Force
```

Run as administrator.
