# Windows Server Administration

## Clean up SoftwareDistribution folder

    net stop wuauserv
    rmdir C:\Windows\SoftwareDistribution /S /Q
    net start wuauserv
