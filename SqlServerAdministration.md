# SQL Server Administration

## Install SQL Server 2008 R2 Express on Windows Server Core 2012 R2

```powershell
Add-WindowsFeature Net-FrameworkCore
```

```powershell
.\SQLEXPRADV_x64_ENU.exe
```

```powershell
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

```sql
with fs
as
(
    select database_id, type, size * 8 / 1024 / 1024 size
    from sys.master_files
)
select 
    name,
    (select sum(size) from fs where type = 0 and fs.database_id = db.database_id) DataFileSizeGiB,
    (select sum(size) from fs where type = 1 and fs.database_id = db.database_id) LogFileSizeGiB
from sys.databases db
order by db.name
```

Source: [StackOverflow](https://stackoverflow.com/a/5946134/991267)

All files:

```sql
select d.name [DB Name], m.name [File Name], m.size * 8 / 1024 / 1024 [Size, GiB]
from sys.master_files m JOIN sys.databases d ON d.database_id = m.database_id and m.type in (0, 1);
```

Source: [StackOverflow](https://stackoverflow.com/a/43472838/991267)

## Shrink database

```sql
DBCC SHRINKDATABASE (DatabaseName)
```

Check progress:

```sql
SELECT command,
            s.text,
            start_time,
            percent_complete,
            CAST(((DATEDIFF(s,start_time,GetDate()))/3600) as varchar) + ' hour(s), '
                  + CAST((DATEDIFF(s,start_time,GetDate())%3600)/60 as varchar) + 'min, '
                  + CAST((DATEDIFF(s,start_time,GetDate())%60) as varchar) + ' sec' as running_time,
            CAST((estimated_completion_time/3600000) as varchar) + ' hour(s), '
                  + CAST((estimated_completion_time %3600000)/60000 as varchar) + 'min, '
                  + CAST((estimated_completion_time %60000)/1000 as varchar) + ' sec' as est_time_to_go,
            dateadd(second,estimated_completion_time/1000, getdate()) as est_completion_time
FROM sys.dm_exec_requests r
CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) s
WHERE r.command in ('RESTORE DATABASE', 'BACKUP DATABASE', 'RESTORE LOG', 'BACKUP LOG', 'DbccFilesCompact')
```

Source: [MSDN Forums](https://social.msdn.microsoft.com/Forums/sqlserver/en-US/2c8eca51-6259-4a51-bf42-8bfe1c688a58/i-want-estimatedcompletiontime-value-in-seconds?forum=sqldatabaseengine)
