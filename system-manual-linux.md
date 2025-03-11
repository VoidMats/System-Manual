## Table of content

* [Get linux operation version](#get-linux-operation-version)
* [Get hardware info on a linux system](#get-hardware-info-on-a-linux-system)
    * [Get network info on a linux system](#get-network-info-on-a-linux-system)
    * [Check mac address on IPs close to this computer](#check-mac-address-on-ips-close-to-this-computer)
    * [List port on linux system](#list-port-on-a-linux-system)
    * [Create alias on a linux system](#create-alias-in-linux)
* [Install/Uninstall .deb](#installuninstall-deb-package)
* [Network](#network)
    * [Get network info](#get-network-info-on-a-linux-system)
    * [NetworkManager](#networkmanager)
* [Firewall](#firewall)
* [Chron](#chron-daemon-jobs)
    * [Chron syntax](#basic-syntax)
* [Logs](#log--loggers)
* [SSH](#ssh)
    * [SSH configuration](#ssh-configuration)
    * [File transfer, remote/client - scp](#scp-file-transfer-through-ssh)

### Linux commands

#### Get linux operation version
```
$ cat /etc/os-release                           # Fedora
```

#### Get hardware info on a linux system
```
$ lscpu                                         # CPU info
$ lshw                                          # Possible of installation. Almost all hardware info you can think of
$ lshw -short                                   # More compehendred info
```





#### List port on a linux system
Use grep to show only ports which is/are listening. To stop any ongoing processes, use the 'kill' command  by its id. If the process still does not stop, it's possible to use -9 which is a force signal. But any unsaved data will be lost. It possible to use ***alias*** for complicated commands, see Create alias in linux.
```
$ lsof -i -P -n | grep LISTEN                   # Will list all ports that are listening
$ sudo kill -1 [Id_from_second_column_in_list]  # Will greatfully kill service running on that port
$ sudo kill -9 [Id_from secong_column_in_list]  # Will force kill of service
```

#### Copy current path to clipboard
To copy content from a file or a terminal command to the clipboard, there is no command for it, in the linux core. However, very often ***xclip*** or ***xsel*** is used for this purpose. In this guide only xclip will be used. To check if it's installed type:
```
$ which xclip
```
With xclip it will become easy to pipe ***pwd*** to clipboard. The first command is used, when using middle mouse button to paste content. If you use ctrl+shift+v use the second command. 
```
$ pwd | xclip -o
$ pwd | xclip -o -sel clip
$ xclip -sel clip [filename]
```

#### Create alias in linux
If the file **~/.bashrc** have the following lines 
```
if [ -f ~/.bash_aliases ]; then
. ~/.bash_aliases
fi
```
add aliases in file **~/.bash_aliases**, otherwise just place it into the **~/.bashrc**. Example on aliases
```
alias grep='grep --color=auto'
alias ls-p='lsof -i -P -n'
alias ls-lp='lsof -i -P -n | grep LISTEN'
alias copy-pwd='pwd | xclip -o -sel clip'
alias copy-file='xclip -sel clip'
alias paste='xclip -sel clip -o'
```

Reactivate the **.bashrc** file 
```
$ . ~/.bashrc                                   # Both command will due
$ source ~/.bashrc 
```

#### System disk information
```
$ lsblk                                         # 
$ df -h                                         #
```

### Install/Uninstall .deb package
Verify that the file does not have any external changes. For this you will need an SHA-256 file which could be used to do a checksum. This will of course not 100% garantie that the file is valid. 
```
  To get the hash 
$ sha256sum ~/path/to/file.zip
  This should be compared with the SHA-256 from the supplier
$ sha256sum ~/path/to/file.zip | grep <supplied hash>
```


```
  # Install
$ sudo dpkg -i package_name.deb

  # Uninstall
$ sudo apt remove package_name

```

### Network

#### Get network info on a linux system.
It should be noted depending on which linux os is running the commands below might not work. Also, it's very depending of which version is used. Few of many commands...
```
$ ifconfig                                      # Lista all network interfaces that are operative
$ cat /etc/network/interfaces                   # Possible that configuration file is placed in another location
$ ip -c a                                       # Print all network interfaces. Very handy command
$ ip addr                                       # Simalar to ifconfig
$ ip route list default | grep dhcp             # Check if your ip is dynamic
$ ip route list default | grep static           # Check if your ip is static
$ ip link show [connection]                     # Check status of connection/NIC
$ ip neighbour                                  # Check mac address closes to this IP
```
#### ethtool

```
$ sudo ethtool --set-eee [connection] off       # Disable power saving - EEE
$ sudo ethtool --show-eee [connection]          # Show if EEE supported  
```

#### NetworkManager
Commands only specific for NetworkManager and NetworkManager-gnome. 
```
$ nmcli device show [connection]                # Realtime log of connection 
$ nmcli con show                                # Show all network connection
$ nmcli con show [connection]                   # Get specific info of connection
```

### Firewall

#### UFW
The default firewall for a linux system is usally 'ufw'. It's a tool to configure the *iptables* of the firewall. Depending on linux destribution the firewall could be disabled. The default policy is 'deny incoming traffic' and 'allow outgoing traffic'. 
```
$ sudo ufw enable                               # Enable firewall on machine
$ sudo ufw status verbose                       # Check status of firewall
$ sudo ufw disable                              # Disable firewall on machine
```
To add a rule is as simple as using the allow/deny command and then reload the ufw. 
```
$ sudo ufw allow 22/tcp                         # Example to allow port 22/tcp
$ sudo ufw allow 22 from [ip]                   # Allow port 22 from ip 
$ sudo ufw deny 23/udp                          # deny traffic on port 23 for tcp
$ sudo ufw reload                               # Apply the rule, by reload the firewall
 Example of deleting a rule
$ sudo ufw delete allow 22/tcp                  # Remove rule
 To make it easier to delete rules run these commands
$ sudo ufw status numbered 
$ sudo ufw delete [number]
```
It's also possible to strict allow/deny ingoing and outgoing traffic, by using the 'in' and 'out' argument. 
```
$ sudo ufw allow in 23/tcp                      # Only allow incomming traffic on port 23
$ sudo ufw deny in 443                          # Deny incoming traffic on port 443

```

---
### Chron daemon (jobs)
The Cron daemon is a built-in Linux utility that execute commands/scripts at a predefined time(s) and intervals. The jobs are executed according to a table in a crontab file. Open a crontab file. It possible that the user could be prompted to select a preferred text editor. Enter the corresponding number, for example, 1 for nano, to open the crontab file. If code is selected it will not remember the path.
```
$ crontab -e                                    # Open by current user
$ crontab -e -u [username]                      # Open for another user
$ crontab -l                                    # Check for active cron jobs
```

#### Basic syntax
Normally the cron job send the output of the job to the user email. However, this is not always a wanted feature for very short range interval jobs. 
If you don't want to Redirect the output of the job by the stream operator (>). For example 
```
0 0 * * * /path/to/script.sh > ~/tmp/output.txt     # Output is saved to output.txt file
0 0 * * * /path/to/script.sh >> ~/tmp/output.txt    # Output is added to output.txt file
0 0 * * * /path/to/script.sh > /dev/null 2>&1       # No output
```

Addding MAILTO='' above the cron job will not send an email to the user. It is ofcourse possible to add another email instead of the system email of the current user.   
```
* * * * * [username] command arg1 arg2...
- - - - -
| | | | |
| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
| | | ------- Month (1 - 12)
| | --------- Day of month (1 - 31)
| ----------- Hour (0 - 23)
------------- Minute (0 - 59)

If username is missing current user will be used.
```
#### Note: 
From cron manual:
>The day of a command's execution can be specified in the following two fields — 'day of month', and 'day of week'. If both fields are restricted (i.e., do not contain the "*" character), the command will be run when either field matches the crent time. For example, "30 4 1,15 * 5" would cause a command to be run at 4:30 am on the 1st and 15th of each month, plus every Friday.

#### Example of different cron intervals
|Cron job                                                  | Description                           |
|:---------------------------------------------------------|:--------------------------------------|
| * * * * * /path/to/script                                |Run every minute                       |
| */15 * * * * /path/to/script                             |Run every 15 minute                    |
| 0 9 1-7 * * [ $(date +\%u) = 7 ] && /path/to/your/script |Run every 1 sunday of the Month 09:00  |
| 0 9-17 * * 1-5 /path/to/script                           |Monday to Friday every hour 09:00-15:00| 
| @reboot /path/to/your/script                             |Run at boot                            |

---
### Log & Loggers

#### Print system log - journalctl
```
$ journalctl
 * -a | --all = Show all fields in full. 
 * -f | --follow = Show only the recent journal entries and continue to print.
 * -u | --unit = unit|pattern. Show messages for a specific system unit or service. 
 * -n | --lines = Print the n last lines.
 * -g | --grep = Filter to output with matching words according to "grep" command. 
 * --no-pager = Do not pipe output into a pager
 * --since = [YYYY-MM-DD hh:mm] Gather log from 

 Example: 
$ journalctl --no-pager -n [nr_lines] -u [service]
```
It's of course possible to pipe with grep.  

#### Print kernal log - dmesg
```
$ dmesg
* -T | --ctime = Human readable timestamp
* -P | --no-pager = Do not pipe output into a pager
* -w | --follow = Wait for next messages

 Example:
 $ dmesg | grep -i enp2s0
```

---
### SSH

Create a new key for your system. Regarding algorithm, there are many out there. RSA (SHA-1) was for many years the default in OpenSSH. However, from version 8.2 it was deprecated. It is of course possible to use RSA SHA-2 (RFC8332) that use a higher bit rate. But many people have moved over to ECDSA or DSA. ECDSA is newer asymmetric encryption algorithm that is based on elliptic curves. 
```
$ ssh-keygen -t [algorithm] -C [email]
```
Some of the options are
```
-b = Specifies the number of bits in the key to create (bits). 
-A = Generate host keys of all default key types (rsa, ecdsa and ed25519) if they do not already exist. 
-f = Specifies the filename of the key file.
-t = Specifies the type of key to create. Following types exist: dsa, ecdsa, ecdsa-sk, ed25519,  ed25519-sk, rsa.
```

```
Enter file in which to save the key (/home/[account]/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):  
Enter same passphrase again:
Your identification has been saved in /home/[account]/.ssh/id_rsa.
Your public key has been saved in /home/[account]/.ssh/id_rsa.pub.
The key fingerprint is: SHA256:Up6KjbnEV4Hgfo75YM393QdQsK3Z0aTNBz0DoirrW+c
The key's randomart image is:

+---[RSA 2048]----+ 
|    .      ..oo..| 
|   . . .  . .o.X.| 
|    . . o.  ..+ B| 
|   .   o.o  .+ ..| 
|    ..o.S   o..  | 
|   . %o=      .  | 
|    @.B...     . |
|   o.=. o. . .  .| 
|    .oo  E. . .. | 
+----[SHA256]-----+

  Example:
$ ssh-keygen -t ECDSA
$ ssh-keygen -t rsa -b 2048
```

Add your private SSH key to ssh-agent. Start with activate the agent in the background 
```
$ eval "$(ssh-agent -s)"                                # Activate ssh-agent
$ ssh-add ~/.ssh/[key]                                  # Add key to ssh-agent 
$ ssh-copy-id -i ~/.ssh/[key] [name]@[ip]               # Copy key to another system
```

Delete old known_hosts which gave remote host identification has changed. It possible to check which lines of the file ~/.ssh/known_hosts are effected with command 'cat ~/.ssh/known_hosts'.  
```
$ ssh-keygen -R [hostname|ip]

 Example:
$ ssh-keygen -R 192.168.1.3
```

### SSH configuration
It is possible to "save" different ssh connection by using config (same name) file in the the .ssh folder. By default, it will try to use key .ssh/id_rsa and after that .ssh/id_dsa. It is possible to specify The structure of the file could for example look like this. 
```
Host dev
    HostName dev.domain.com
    User UserName1
    Port 2322

Host alpha
    HostName 192.168.10.50
    User UserName2
    port 2323
    LogLevel INFO

Host beta
    HostName 192.168.10.51
    User UserName3
    IdentityFile ~/.ssh/id_rsa
    port 2324
```
To use any configuration write (example from configuration above):
```
$ ssh dev
```

Some of the options 
* LogLevel = Specify level of verbose message. Level which exist are QUIET, FATAL, ERROR, INFO, VERBOSE, DEBUG, DEBUG1, DEBUG2, DEBUG3
* SendEnv = Specify what environment variables to pass forward to the remote machine.
* IdentityFile = Specify which key (file) to use against user.
* HostKeyAlgorithms = 
* ForwardAgent = The connection to the authentication agent will be forward to the remote machine.
* Compression = Messages will be compressed.
* ConnectionAttempts = Set the number of attempts before exiting.

### SCP (file transfer through ssh)
To transfer a file(s) between a remote and client it's easiest to use scp. It should be noted to be careful when writting files...

> Note 1: The default port for scp is 22. If the remote server is using another port, this has to be specified in the command. See example 3.

> Note 2: 
```
$ scp [options] [source_user]@source_host:/directory/source_file [destination_user]@destination_host:/directory(/destination_file)
 
 Examples:

Copy from your local pc to a remote server
$ scp image.png remote_user1@192.168.1.2:/desktop/images

Copy from one remote server to another 
$ scp remote_user1@192.168.1.2:/desktop/images/image.png remote_user2@192.168.1.3/download

Copy local file to remote server on port 24
$ scp ./foo.txt remote_user1@192.168.1.2:24/desktop/images/foo.txt
```
Some of the options are
```
-P = Specify which remote host ssh port to use.
-p = Preserves file modification and access time.
-q = Suppress progress meter and non-error messages
-C = Compress data during transfer
-r = Copy directories recursively
```
---

### Mount on linux

#### CIFS
Make sure that CIFS utilities packages are installed.
```
$ sudo apt update                               # Or similar depening on os.
$ sudo apt install cifs-utils                   # Ubuntu/Debian
$ sudo dnf install cifs-utils                   # CentoOS/Fedora/Red hat
```
Make sure there is a folder where you want the mount. Usually all mounts are placed under /mnt/[folder]. 
```
$ sudo mount -t cifs -o username=[user],password=[pwd] //ip-server/path/folder /mnt/created_folder
```
Its possible to test or 'fake' a mount by using the flag --fake. It's recommended to also use the --verbose for full logging. This also a good way to check mount setting in *fstab* by running.
```
$ sudo mount --fake -av
```
To release a mount use the *unmount* command.
```
$ sudo unmount /mnt/path/folder
```

### npm

Install new repository on a windows system and get following error: ENOTSUP: operation not supported on socket, symlink
```
$ npm install --no-bin-links                                # npm does not create symlinks for any binaries 
```
Sometime git go nuts on package-lock.json and its depencencies. If it's not possible to resolve, delete package-lock.json (**Note!** only package-lock.json) and create a new one from package.json
```
$ npm install --package-lock-only                           # npm create a new package-lock.json
```

Install a private repository. More info [npm doc - url dependencies](https://docs.npmjs.com/cli/v8/configuring-npm/package-json#git-urls-as-dependencies) and [npm doc - private packages](https://docs.npmjs.com/about-private-packages)
```
<protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>[#<commit-ish> | #semver:<semver>]

Example
$ npm install git+ssh://git@bitbucket.org/user/repository.git       # Install repo from bitbucket
$ npm install git+ssh://git@github.com/user/project.git             # Install repo from github
$ npm install git+ssh://user@hostname:project.git#commit-ish        # Install for a certain commit
$ npm install git+https://user@hostname/project/blah.git            # Install with https

Concrete examples
$ npm install git+ssh://git@github.com:npm/cli.git#v1.0.27
$ npm install git+ssh://git@github.com:npm/cli#semver:^5.0
$ npm install git+https://isaacs@github.com/npm/cli.git
$ npm install git://github.com/npm/cli.git#v1.0.27
```

### yarn


### Certificate

#### Check private key
> openssl rsa -modulus -noout -in private.pem | openssl md5
#### Check cerifificate key
> openssl x509 -modulus -noout -in certificate.pem | openssl md5
#### Check valid date certificate 
> openssl x509 -enddate -noout -in certificate.pem

### Sqlite3

Check if database is malformed. The check is done on following:
- Table and index entries that are out of sequence.
- Misformatted records.
- Missing or surplus index entries.
- UNIQUE, CHECK, NOT NULL contain errors.
- Integrity of the freelist.
- Sections of the database that are used more than once, or not at all. 
```
$ sqlite3 mydata.db "PRAGMA integrity_check"
```

Repair malformed database
```
$ sqlite3 broken.db ".recover" | sqlite3 new.db         #
$ sqlite3 broken.db ".dump" | sqlite3 new.db            # 
```

### PostgreSQL

Login to PostgresSQL in terminal
```
$ psql <protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>
$ psql -h [hostname] -d [database] -U [user] -p [port]

Example:
$ psql postgresql://zabbix:<pwd>@localhost:5432/zabbix
```

Vacuum system. When running full any service which use this database should be shutdown during this process.
```
$ VACUUM (VERBOSE, ANALYZE, FULL) [table];
```
 - VERBOSE = Prints detailed vacuum activity
 - ANALYZE = Updates statistics used by the planner to determine the most efficient way to execute a query
 - FULL = Reclaim more space to the system by rewritting the table. Need double the space as the table during this process.

## Service specific commands

### Teleport 

Start and stop teleport
```
sudo service teleport start 
sudo systemctl start teleport
sudo service teleport stop
sudo systemctl stop teleport
```

### Zabbix

Start and stop zabbix
```
$ sudo systemctl stop zabbix-server             # Shutdown zabbix
$ sudo systemctl start zabbix-server            # Start zabbix
```


