# Windows Server Administration

## Clean up SoftwareDistribution folder

    net stop wuauserv
    rmdir C:\Windows\SoftwareDistribution /S /Q
    net start wuauserv

Source: [Microsoft Community](https://answers.microsoft.com/en-us/windows/forum/windows_xp-files/is-it-safe-to-delete-files-under/8d98d924-b0b1-4f2b-bb4b-13f38126c588)

## Read Event Log

[PowerShell](https://msdn.microsoft.com/en-us/powershell/reference/4.0/microsoft.powershell.management/get-eventlog):

```
Get-EventLog Application | select -First 100
```

[wevtutil](https://technet.microsoft.com/en-us/library/dd310329.aspx):

```
wevtutil qe Application /c:100 /f:text
```

## Create Network Share

```
New-Item C:\Workspace -ItemType Directory
New-SmbShare -Name Workspace -Path C:\Workspace -FullAccess administrator
```

```
Remove-SmbShare Workspace
```

Source: [I Love PowerShell](http://ilovepowershell.com/2012/09/19/create-network-share-with-powershell-3/)
[Documentation](https://technet.microsoft.com/itpro/powershell/windows/smbshare/new-smbshare)
