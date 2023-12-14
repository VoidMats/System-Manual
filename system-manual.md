

## System specific commands

### Linux commands
Get linux/operation version
> cat /etc/os-release

Get network info
> ifconfig

> cat /etc/network/interfaces

> ip -c a

Check if your ip is static
> ip a | grep dynamic

List ports on system. Use grep to 
> sudo lsof -i -P -n [ | grep LISTEN ]

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
> journalctl --no-pager -n [ nr_lines ] -u [ service ]


### SSH

Create a new key for your system
> ssh-keygen -t [ algorithm ] -C [ email ]

Add your private SSH key to ssh-agent. Start with activate the agent in the background 
> eval "$(ssh-agent -s)"

> ssh-add ~/.ssh/[key]

Copy key to another system
> ssh-copy-id -i ~/.ssh/[key] [name]@[ip]

Delete old known_hosts which gave remote host identification has changed. It possible to check which lines of the file ~/.ssh/known_hosts are effected with command 'cat ~/.ssh/known_hosts'.  
> ssh-keygen -R [hostname|ip]

### Certificate

#### Check private key
> openssl rsa -modulus -noout -in private.pem | openssl md5
#### Check cerifificate key
> openssl x509 -modulus -noout -in certificate.pem | openssl md5
#### Check valid date certificate 
> openssl x509 -enddate -noout -in certificate.pem

### SQLITE3

Check if database is malformed
> sqlite3 mydata.db "PRAGMA integrity_check"

Repair malformed database
> sqlite3 broken.db ".recover" | sqlite3 new.db

> sqlite3 broken.db ".dump" | sqlite3 new.db


## Service specific commands

### Teleport 

Start and stop teleport
> sudo service teleport start 

> sudo systemctl start teleport

> sudo service teleport stop

> sudo systemctl stop teleport

