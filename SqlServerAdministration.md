# SQL Server Administration

## Install SQL Server 2008 R2 Express on Windows Server Core 2012 R2

```
Add-WindowsFeature Net-FrameworkCore
```

```
.\SQLEXPRADV_x64_ENU.exe
```

```
# Enable TCP/IP

Get-CimInstance -Namespace root/Microsoft/SqlServer/ComputerManagement10 -ClassName ServerNetworkProtocol -Filter "InstanceName = 'SQLEXPRESS' and ProtocolName = 'Tcp'" |
Invoke-CimMethod -Name SetEnable

# Open the right ports in the firewall
New-NetFirewallRule -DisplayName 'MSSQL$SQLEXPRESS' -Direction Inbound -Action Allow -Protocol TCP -LocalPort 1433

# Modify TCP/IP properties to enable an IP address

$properties = Get-CimInstance -Namespace root/Microsoft/SqlServer/ComputerManagement10 -ClassName ServerNetworkProtocolProperty -Filter "InstanceName='SQLEXPRESS' and ProtocolName = 'Tcp' and IPAddressName='IPAll'"
$properties | ? { $_.PropertyName -eq 'TcpPort' } | Invoke-CimMethod -Name SetStringValue -Arguments @{ StrValue = '1433' }
$properties | ? { $_.PropertyName -eq 'TcpPortDynamic' } | Invoke-CimMethod -Name SetStringValue -Arguments @{ StrValue = '' }

# Restart SQL Server

Restart-Service 'MSSQL$SQLEXPRESS'
```

Source: [StackOverflow](http://stackoverflow.com/a/44083124/991267)
