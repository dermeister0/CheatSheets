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

## Get size of all data and log files

```
with fs
as
(
    select database_id, type, size * 8.0 / 1024 size
    from sys.master_files
)
select 
    name,
    (select sum(size) from fs where type = 0 and fs.database_id = db.database_id) DataFileSizeMB,
    (select sum(size) from fs where type = 1 and fs.database_id = db.database_id) LogFileSizeMB
from sys.databases db
```

Source: [StackOverflow](https://stackoverflow.com/a/5946134/991267)
