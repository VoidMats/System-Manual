## System specific commands

### Linux commands
Get linux/operation version
```
$ cat /etc/os-release                           # 
```

Get network info on a linux system. It should be noted depending on which linus os is running the commands below might not work. Also, it's very depending of which version is used. Few of many commands...
```
$ ifconfig                                      # Lista all network interfaces that are operative
$ cat /etc/network/interfaces                   # Possible that configuration file is placed in another location
$ nmcli con show                                # Show all network connection
$ nmcli con show [connection]                   # Get specific info of connection
$ ip -c a                                       #
$ ip route list default | grep dhcp             # Check if your ip is dynamic
$ ip route list default | grep static           # Check if your ip is static
```

Check mac address on IPs close to this computer
```
$ ip neighbour
```

List ports on system. Use grep to show only ports which is/are listening. To stop any ongoing processes, use the 'kill' command  by its id. If the process still does not stop, it's possible to use -9 which is a force signal. But any unsaved data will be lost.
```
$ sudo lsof -i -P -n | grep LISTEN                      # Will list all ports that are listening
$ sudo kill -1 [Id_from_second_column_in_list]          #      
```

System disk information
```
$ lsblk                                                 # 
$ df -h                                                 #
```

---
### Log & Loggers

#### Print system log 
> journalctl
 * -a | --all = Show all fields in full. 
 * -f | --follow = Show only the recent journal entries and continue to print.
 * -u | --unit = unit|pattern. Show messages for a specific system unit or service. 
 * -n | --lines = Print the n last lines.
 * -g | --grep = Filter to output with matching words according to "grep" command. 
 * --no-pager = Do not pipe output into a pager

Example: 
> journalctl --no-pager -n [nr_lines] -u [service]

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
> ssh-keygen -R [hostname|ip]
> example $ ssh-keygen -R 192.168.1.3

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

### Docker & docker-compose

To use a terminal within a container
```
$ sudo docker exec -it [kontainernamn] sh 
$ sudo docker-compose exec [service_name_in_docker_compose] sh
```
See actual status of all containers in a docker system. Update interval 
```
$ sudo docker stats
$ sudo docker container stats [CONTAINER...]
```
See how much disk space could be reclaimed from docker and how to reclaim it. It should be noted that prune will remove all containers which are in stop state.  
```
$ sudo docker system df
$ sudo docker system prune
``` 

### npm

Install new repository on a windows system and get following error: ENOTSUP: operation not supported on socket, symlink
```
$ npm install --no-bin-links                                # Npm does not create symlinks for any binaries 
```
Sometime git go nuts on package-lock.json and its depencencies. If it's not possible to resolve, delete package-lock.json (**Note!** only package-lock.json) and create a new one from package.json
```
$ npm install --package-lock-only                           # Npm create a new package-lock.json
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
- UNIQUE, CHECK, NOT NULL constrain errors.
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

Vacuum system. When running full any service which use this database should be shutdown during this process.
```
$ VACUUM (VERBOSE, ANALYZE, FULL) table;
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
> sudo systemctl stop zabbix-server

> sudo systemctl start zabbix-server


