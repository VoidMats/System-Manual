
## Table of content


### Windows commands


### Config powershell
To config powershell it must have a profile. By default it's not possible to create a profie, but it's possible to activate it by running this in powershell. 
```
$ Get-executionpolicy                                               # Show execution policy of scripts in powershell
$ Set-executionpolicy -ExecutionPolicy [option] -Scope [option]     # Set execution policy for powershell

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

### Set python on another folder 
There are several options to run python from another folder. Python must be installed inside this folder. Could be that symlink is not working properly. 

1. Create a symlink to the folder
2. Create a batch file 

#### Create symlink
For command prompt (as administrator)
```
$ mklink C:\Windows\System32\python3.exe C:\path\to\folder\python.exe
```
Validate in command prompt
```
$ dir C:\Windows\System32\python3.exe
```
For powershell
```
$ New-Item -ItemType SymbolicLink -Path "C:\Windows\System32\python3.exe" -Target "C:\path\to\folder\python.exe"
```
Validate in powershell
```
$ Get-Item "C:\Windows\System32\python3.exe"
```

#### Create batch file 
Create a file with the following content
```
@echo off
"C:\path\to\folder\python.exe" %*
```
Save the file as python3.bat in the path "C:\Windows\System32". Make sure the environment system variable PATH has the path "C:\Windows\System32". Validate with the command "python3 --version".

## SSH

### Copy SSH key to remote server
There are several ways to do this. If you are using a wsl environment, it's important that the right ssh key is copied. 
To copy ssh key in wsl use the linux command ssh-copy-id
```
$ ssh-copy-id -i ~/.ssh/[key].pub [user]@[remote-host]
ex: ssh-copy-id -i ~/.ssh/id_rsa.pub user@192.168.1.2
```
To copy ssh key from windows 
```
$ ssh-copy-id -i /mnt/c/Users/your-user/.ssh/[key].pub [user]@[remote-host]
ex: ssh-copy-id -i /mnt/c/Users/your-user/.ssh/id_rsa.pub user@192.168.1.
```
