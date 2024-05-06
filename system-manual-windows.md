
## Table of content


### Windows commands


### Config powershell
To config powershell it must have a profile. Activate profile by running this in powershell. 
```
$ Get-executionpolicy                   # Show execution policy of scripts in powershell
$ Set-executionpolicy -ExecutionPolicy [option] -Scope [option]

 Scope options
MachinePolicy = policy on all users
UserPolicy = Set policy on user
Process = The Process scope only affects the current PowerShell session
CurrentUser =  Set policy on current user
LocalMachine = The execution policy affects all users on the current computer. It's stored in the HKEY_LOCAL_MACHINE

 Execution options
Restricted = Not allowed. Default
Bypass = Allowed. No warning
Unrestricted = Allowed. But prompt for permission
AllSigned = Requires that all scripts and configuration files are signed by a trusted publisher
RemoteSigned = Requires that all scripts and configuration files downloaded from the Internet are signed by a trusted publisher
```
More info: [windows manual](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.4) 
