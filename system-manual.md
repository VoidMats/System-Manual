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

Check mac address on IPs close to this computer
> ip neighbour

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


### SSH

Create a new key for your system
> ssh-keygen -t [algorithm] -C [email]

Add your private SSH key to ssh-agent. Start with activate the agent in the background 
> eval "$(ssh-agent -s)"

> ssh-add ~/.ssh/[key]

Copy key to another system
> ssh-copy-id -i ~/.ssh/[key] [name]@[ip]

Delete old known_hosts which gave remote host identification has changed. It possible to check which lines of the file ~/.ssh/known_hosts are effected with command 'cat ~/.ssh/known_hosts'.  
> ssh-keygen -R [hostname|ip]
> example $ ssh-keygen -R 192.168.1.3

### Docker & docker-compose

Login to a container
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

### NPM

Install new repository on a windows system ()
```
$ npm install -no-bin-links
```

### Certificate

#### Check private key
> openssl rsa -modulus -noout -in private.pem | openssl md5
#### Check cerifificate key
> openssl x509 -modulus -noout -in certificate.pem | openssl md5
#### Check valid date certificate 
> openssl x509 -enddate -noout -in certificate.pem

### Sqlite3

Check if database is malformed
> sqlite3 mydata.db "PRAGMA integrity_check"

Repair malformed database
> sqlite3 broken.db ".recover" | sqlite3 new.db

> sqlite3 broken.db ".dump" | sqlite3 new.db

### PostgreSQL

Vacuum system. When running full any service which use this database should be shutdown during this process.
> VACUUM (VERBOSE, ANALYZE, FULL) table;
* VERBOSE = Prints detailed vacuum activity
* ANALYZE = Updates statistics used by the planner to determine the most efficient way to execute a query
* FULL = Reclaim more space to the system by rewritting the table. Need double the space as the table during this process.  

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


