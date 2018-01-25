# Windows Server Administration

## Clean up SoftwareDistribution folder

    net stop wuauserv
    rmdir C:\Windows\SoftwareDistribution /S /Q
    net start wuauserv

Source: [Microsoft Community](https://answers.microsoft.com/en-us/windows/forum/windows_xp-files/is-it-safe-to-delete-files-under/8d98d924-b0b1-4f2b-bb4b-13f38126c588)

## Read event log

[PowerShell](https://msdn.microsoft.com/en-us/powershell/reference/4.0/microsoft.powershell.management/get-eventlog):

```
Get-EventLog Application | select -First 100
```

[wevtutil](https://technet.microsoft.com/en-us/library/dd310329.aspx):

```
wevtutil qe Application /c:100 /f:text
```

## Create network share


```
New-Item C:\Workspace -ItemType Directory
New-SmbShare -Name Workspace -Path C:\Workspace -FullAccess administrator
```

```
Remove-SmbShare Workspace
```

Source: [I Love PowerShell](http://ilovepowershell.com/2012/09/19/create-network-share-with-powershell-3/)
[Documentation](https://technet.microsoft.com/itpro/powershell/windows/smbshare/new-smbshare)

## Get disk free space

```
Get-PSDrive C | Select-Object Used, Free
```

Source: [StackOverflow](http://stackoverflow.com/a/29992160/991267)

## Add permanent environment variable

```
$additionalPath = 'C:\ProgramData\chocolatey\bin;C:\Program Files\Git\cmd'
$currentValue = [Environment]::GetEnvironmentVariable('Path', 'Machine')
$currentValue = [Environment]::GetEnvironmentVariable('Path', [System.EnvironmentVariableTarget]::Machine)
[Environment]::SetEnvironmentVariable('Path', "$currentValue;$additionalPath", [System.EnvironmentVariableTarget]::Machine)
```

Source: [StackOverflow](http://stackoverflow.com/a/2571200/991267)

## Update environment variable from registry

```
$env:PATH = (@(,[Environment]::GetEnvironmentVariable('PATH', 'Machine') -Split ';') + @([Environment]::GetEnvironmentVariable('PATH', 'User') -Split ';') | Select -Unique) -Join ';'
```

## Change drive letter

```
$drive = Get-CimInstance Win32_Volume -Filter "DriveLetter = 'D:'"
Set-CimInstance $drive -Arguments @{DriveLetter='E:'}
```


Source: [Scripting Guy](https://blogs.technet.microsoft.com/heyscriptingguy/2011/03/14/change-drive-letters-and-labels-via-a-simple-powershell-command/)

## Create and format volume on RAW partition

```
Get-Disk | where PartitionStyle -eq 'RAW' | Initialize-Disk -PartitionStyle GPT -PassThru | New-Partition -A
ssignDriveLetter -UseMaximumSize | Format-Volume -FileSystem NTFS -Confirm:$false
```

Source: [Scripting Guy](https://blogs.technet.microsoft.com/heyscriptingguy/2013/05/29/use-powershell-to-initialize-raw-disks-and-to-partition-and-format-volumes/)

## Disable Windows Defender real-time protection

```
Set-MpPreference -DisableRealtimeMonitoring $true
```

Source: [Thomas Maurer](http://www.thomasmaurer.ch/2016/07/how-to-disable-and-configure-windows-defender-on-windows-server-2016-using-powershell/)

## Check license status

```
# Display license information.
slmgr.vbs /dli
```

```
# Display expiration ddate for current license state.
slmgr.vbs /xpr
```

Source: [SQL Panda](http://www.sqlpanda.com/2012/07/check-windows-server-core-license.html)

## Show Windows version

```
Get-CimInstance Win32_OperatingSystem | select Caption, Version, BuildNumber

Get-ComputerInfo -Property Windows*
```

Source: [StackOverflow](https://stackoverflow.com/a/23622106/991267)
