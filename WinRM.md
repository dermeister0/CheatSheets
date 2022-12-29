# WinRM

Add a host to the trusted list for NTLM connection:

```
Set-Item WSMan:\localhost\Client\TrustedHosts example.local -Concatenate -Force
```
