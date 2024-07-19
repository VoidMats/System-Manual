
## Git

* [General](#general)
    - [Set upstream on a branch](#set-upstream-on-a-branch)
    - [Change origin to another url](#change-origin-to-another-url)
* [Tag](#tag)
* [Config](#config)
    - [Set upstream as default on push](#set-config-to-always-place-upstream-on-push)

### General

#### Set upstream on a branch
```
$ git branch --set-upstream-to=origin/<branch> branch

$ git push --set-upstream origin <branch>       # Possible to set upstream during push 

# NB! Please note. This could be set in config file 
```
#### Change origin to another url
```
$ git remote -v                                             # First check existing url
> Example of how it could look like
> origin  https://github.com/user/repo.git (fetch)
> origin  https://github.com/user/repo.git (push)

$ git remote set-url origin https://new.url/user/repo.git   # Change url on origin

$ git remote show origin                                    # Confirm changes
```

### Config
Git config set confiuration values on a system, global, and local level. The most important values when you create a new repository is to set user.name and user.mail before making the first commit to the repository. As following...
``` 
$ git config user.name "Firstname Secondname"
$ git config user.mail "some_mail@example.com"
```
#### Get or list configuration options
To list or get key-values from the configuration file, there are many command options to your disposile

```
$ git config --list                     # List all configuration options
$ git config --get user.name            # Get value for configuration user.name locally
$ git config --get-all user.name        # List all values for the configuration user.name (locally, globally, system)
$ git config --get-regexp user          # Get values according to a regex
```

#### Config levels
**Local**: [--local] Configuration will be stored loaclly for the repository where the command is performed upon. This is also the default option when no configuration level has been passed. The configuration file is stored in the same directory as the repository. 

**Global**: [--global] Global level configuration which onyl apply to the user. Global configuration file(s) is/are stored localy on user home directory. For example on a unix system *~/.gitconfig*

**System**: [--system] System level configuration which is applied accross an entire machine. This mean it will effect all users and repositories. The values are stored in *gitconfig* at the system root path. 

#### Set editor to use during merge conflicts
All commands below are set on a global level, meaning effect only the user. 

|Edit               |Command                                            |
|:------------------|:--------------------------------------------------|
| Atom              | git config --global core.editor "atom --wait"     |
| emacs             | git config --global core.editor "emacs"           |
| nano              | git config --global core.editor "nano -w"         |
| vim               | git config --global core.editor "vim"             |

#### Set config to always place upstream on push
```
$ git config --global --add --bool push.autoSetupRemote true
```
#### Set merge options
If the repository is new and there is no global configuration for merge. Use one of following options. Git will actually report back these options if there is none. 
```
git config pull.rebase false  # merge (the default strategy)
git config pull.rebase true   # rebase
git config pull.ff only       # fast-forward only
```


As usal more info can be found [git doc](https://git-scm.com/docs/git-config)

### Tag
There are two types of tages, *annotated* and *lightweight* tags. To create a new tag run following command
```
$ git tag <tagname> [options] 

 Options
-a = Create an annotated tag which add more metadata, such as email, date, name, to the tag
-m = Add a message to the tag
-d = Delete a tag
-f = Force to replace an already existing tag
-l = List all tags

 Example
$ git tag v1.2.3                                # Create a lightweighted tag
$ git tag -a v1.2.4 -m "Tag message"            # Create an annotated tag
$ git tag -a -f v1.2.4 -m "Replace tag"         # Replace tag v1.2.4
$ git tag -d v1.2.4                             # Delete tag
$ git push tag -d origin v1.2.4                 # Delete tag on remote
$ git push origin :refs/tags/v1.2.4             # Delete tag on remote (alternative way)
$ git checkout tag 1.2.3                        # Checkout to commit with tag v1.2.3
```
Sharing a tag is the same as pushing a branch. However, pushing a branch does not push the tag(s) connected to it. So, this has to be done explicitly by pushing it tag name, such as example.
```
$ git push origin v1.2.3
```

More info: [git doc](https://git-scm.com/docs/git-tag)

## SVN
Most important to know is that SVN is a central rep
### Basic commands

#### Checkout
Check out a working copy from the repository. This command is sometimes shortened to svn co. Path could be omitted
```
$ svn checkout [url/path]                       # Checkout a complete repository 

```

#### Commit
Commit code and report back changes to the repository.
```
$ svn commit                                    # Commit code to the repository

 Options
-m = Message
```

#### List
