
## Git 

* [Tag](#tag)
* [Config](#config)

### Config
Git config set confiuration values on a system, global, and local level. The most important values when you create a new repository is to set user.name and user.mail before making the first commit to the repository. As following...
``` 
$ git config user.name "Firstname Secondname"
$ git config user.mail "some_mail@example.com"
```

#### Config levels
**Local**: [--local] 

**Global**: [--global] 

**System**: [--system] System-level configuration which is applied accross an entire machine. This mean it will effect all users and repositories. The values are stored in *gitconfig* at the system root path. 

#### Set editor to use during merge conflicts

|Edit               |Command                                            |
|:------------------|:--------------------------------------------------|
| Atom              | git config --global core.editor "atom --wait"     |
| emacs             | git config --global core.editor "emacs"           |
| nano              | git config --global core.editor "nano -w"         |
| vim               | git config --global core.editor "vim"             |

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
$ git checkout tag 1.2.3                        # Checkout to commit with tag v1.2.3
```
Sharing a tag is the same as pushing a branch. However, pushing a branch does not push the tag(s) connected to it. So, this has to be done explicitly by pushing it tag name, such as example.
```
$ git push origin v1.2.3
```

More info: [git doc](https://git-scm.com/docs/git-tag)