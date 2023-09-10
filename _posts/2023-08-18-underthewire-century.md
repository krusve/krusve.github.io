century 1 - century1

## Connection

Putty
- Host Name: century.underthewire.tech
- Port: 22
- Username: century1
- Password: century1

### Century 1
- The password for Century2 is the build version of the instance of PowerShell installed on this system.
```Powershell
PS C:\users\century1\desktop> $PSVersionTable
Name                           Value
----                           -----
PSVersion                      5.1.14393.5127
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.14393.5127
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```


century2 - 10.0.14393.5127

### Century 2
- The password for Century3 is the name of the built-in cmdlet that performs the wget like function within PowerShell PLUS the name of the file on the desktop.

- ```dir``` or ```Get-ChildItem``` on desktop to find a file called 443 (Get-Content to read - some information about port 443 but nothing important)
- ```help wget``` to find ```Invoke-WebRequest``` is Powershell equivalent

- Final password: ```invoke-webrequest443```


### Century3

- The password for Century4 is the number of files on the desktop.
- ```Get-ChildItem | Measure-Object```
- Password: 123










## List of all commands
  
- ```$PSVersionTable```: Show Powershell version and Information
- ```Get-ChildItem```: aka dir
- ```Get-Content```: output conntents of a file
- ```Invoke-WebRequest```: Powershell wget
- ```Get-ChildItem | Measure-Object```: Count number of files
