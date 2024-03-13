
## Git 

* [Tag](#tag)

### Tag
There are two types of tages, *annotated* and *lightweight* tags. To create a new tag run following command
```
$ git tag <tagname> [options] 

 Options
-a = Create an annotated tag which add more metadata, such as email, date, name, to the tag
-m = Add a message to the tag
-d = Delete a tag
-f = Force to replace an already existing tag

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