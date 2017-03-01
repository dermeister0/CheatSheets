# Windows Server Administration

## Clean up SoftwareDistribution folder

    net stop wuauserv
    rmdir C:\Windows\SoftwareDistribution /S /Q
    net start wuauserv

Source: [Microsoft Community](https://answers.microsoft.com/en-us/windows/forum/windows_xp-files/is-it-safe-to-delete-files-under/8d98d924-b0b1-4f2b-bb4b-13f38126c588)
