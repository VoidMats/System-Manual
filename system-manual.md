## System specific commands

* [Get linux operation version](#get-linux-operation-version)
* [Get hardware info on a linux system](#get-hardware-info-on-a-linux-system)
* [Get network info on a linux system](#get-network-info-on-a-linux-system)
* [Check mac address on IPs close to this computer](#check-mac-address-on-ips-close-to-this-computer)
* [List port on linux system](#list-port-on-a-linux-system)
* [Create alias on a linux system](#create-alias-in-linux)
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

#### Get network info on a linux system. 
It should be noted depending on which linus os is running the commands below might not work. Also, it's very depending of which version is used. Few of many commands...
```
$ ifconfig                                      # Lista all network interfaces that are operative
$ cat /etc/network/interfaces                   # Possible that configuration file is placed in another location
$ nmcli con show                                # Show all network connection
$ nmcli con show [connection]                   # Get specific info of connection
$ ip -c a                                       #
$ ip addr                                       # Simalar to ifconfig
$ ip route list default | grep dhcp             # Check if your ip is dynamic
$ ip route list default | grep static           # Check if your ip is static
```

#### Check mac address on IPs close to this computer
```
$ ip neighbour
```

#### List port on a linux system
Use grep to show only ports which is/are listening. To stop any ongoing processes, use the 'kill' command  by its id. If the process still does not stop, it's possible to use -9 which is a force signal. But any unsaved data will be lost.
```
$ lsof -i -P -n | grep LISTEN                       # Will list all ports that are listening
$ sudo kill -1 [Id_from_second_column_in_list]      # Will greatfully kill service running on that port
$ sudo kill -9 [Id_from secong_column_in_list]      # Will force kill of service
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
alias ls-port='lsof -i -P -n | grep LISTEN'
```

Reactivate the **.bashrc** file 
```
$ . ~/.bashrc                   # Both command will due
$ source ~/.bashrc 
```


#### System disk information
```
$ lsblk                                                 # 
$ df -h                                                 #
```


---
### Log & Loggers

#### Print system log 
```
$ journalctl
 * -a | --all = Show all fields in full. 
 * -f | --follow = Show only the recent journal entries and continue to print.
 * -u | --unit = unit|pattern. Show messages for a specific system unit or service. 
 * -n | --lines = Print the n last lines.
 * -g | --grep = Filter to output with matching words according to "grep" command. 
 * --no-pager = Do not pipe output into a pager

 Example: 
$ journalctl --no-pager -n [nr_lines] -u [service]
```

---
### ssh

Create a new key for your system
> ssh-keygen -t [algorithm] -C [email]

Add your private SSH key to ssh-agent. Start with activate the agent in the background 
```
$ eval "$(ssh-agent -s)"                                # Activate ssh-agent
$ ssh-add ~/.ssh/[key]                                  # Add key to ssh-agent 
$ ssh-copy-id -i ~/.ssh/[key] [name]@[ip]               # Copy key to another system
```

Delete old known_hosts which gave remote host identification has changed. It possible to check which lines of the file ~/.ssh/known_hosts are effected with command 'cat ~/.ssh/known_hosts'.  
```
$ ssh-keygen -R [hostname|ip]
$ example $ ssh-keygen -R 192.168.1.3
```

#### ssh configuration
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

#### scp (file transfer through ssh)
To transfer a file(s) between a remote and client it's easiest to use scp. It should be noted to be careful when writting files General function
```
$ [options] [source_user]@source_host:/directory/source_file [destination_user]@destination_host:/directory(/destination_file)
 
 Examples:
Copy from your local pc to a remote server
$ scp image.png remote_user1@192.168.1.2:/desktop/images
Copy from one remote server to another 
$ scp remote_user1@192.168.1.2:/desktop/images/image.png remote_user2@192.168.1.3/download
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


