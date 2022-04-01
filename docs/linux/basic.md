# Linux basic commands

## Moving around

### The golden ones
|Command                              | Description
|-------------------------------------|--------------
| `cd ../documents`                   | Change working directory to parent directory and then to the documents directory
| `cp -r origin_dir destination_dir`  | Copy the folder origin_dir into destination_dir
| `ls -lha`                           | List all files (including hidden) in with human-readable size
| `tree -L 3`                         | Show files and folders of the current directory and up to 3 subdirectories
| `pwd`                               | Get absolute path of the current directory
| `chown jhon:students foo.txt`       | Change the owner of foo.txt to the user jhon and to the group students
| `chmod -R ug+r bar`                 | Add read and write permissions to the directory bar and all its contents to the user and the group

### Path references
- Absolute: all the path from the root `/home/user/desktop/class/helloworld.c`
- Relative: from current folder. Supposing your working directory is the desktop `class/helloworld.c`
- Current directory reference `.`
- Parent directory reference `..`
- Home directory reference `~`

### Operating with directories/files
|Command                              | Description
|-------------------------------------|--------------
| `cd foo`                            | Change working directory (cd) to foo
| `cd`                                | Change working directory to the user home
| `cd -`                              | Change working directory to the previous one
| `cp origin.c destination.c`         | Copy the file in the origin path o the destination path
| `cp -r origin_dir destination_dir`  | Copy the folder origin_dir into destination_dir
| `mv foo bar`                        | Move foo to bar, foo and be a file or folder. This can also be used to change file/folder names
| `mkdir foo`                         | Make directory foo
| `mkdir -p foo/bar`                  | Make directory bar inside foo directory, if foo doesn't exist make it as well
| `touch foo.txt`                     | Make an empty file named foo.txt
| `rm foo.txt`                        | Remove the file foo.txt
| `rm -r foo`                         | Remove the directory foo and all of it's contents

### Getting directories/files information
|Command      | Description
|-------------|--------------
| `ls`            | Show contents of the current directory
| `ls foo`        | Show contents of the folder foo
| `ls -l`         | Show contents of the current directory in a list, with permissions, owners, size and date/time
| `ls -lh`        | Same as `ls and-l` but show size in human-readable size (KB, MB, GB, etc.)
| `ls -a`         | Show hidden files
| `ls -S`         | Order by file size, the largest first
| `ls -R`         | List recursively folders contents (I prefer `tree` over this one)
| `tree`          | List recursively folders contents with a nice view
| `tree -a`       | Also list hidden items
| `tree -L X`     | Only list files and folder up to X levels
| `tree -d`       | Only list directories, ignore files
| `pwd`           | Get absolute path of the current directory
| `stat foo.txt`  | Get metadata information of the foo.txt file

## File permissions

### Explanation
Each file and directory is assigned to a user and to a group and has specific permissions for the user (u), the group (g) and others (o). Permissions can be: read (r), write (w) and execute (x). These permissions and their combination can be expressed with numbers and letters as following:

| Letter/s  | Permission/s           | Binary | Decimal
|-----------|------------------------|--------|--------
| --x       | Execute                | 001    | 1
| -w-       | Write                  | 010    | 2
| r--       | Read                   | 100    | 4
| -wx       | Write + execute        | 011    | 3
| r-x       | Read + execute         | 101    | 5
| rw-       | Read + write           | 110    | 6
| rwx       | Read + write + execute | 111    | 7

### Assigning permissions and owners
| Command                 | Description
|-------------------------|--------------
| `chown jhon foo.txt`    |   Change the owner of foo.txt to the user jhon
| `chown pat:usr foo.txt` | Change the owner of foo.txt to the user pat and to the group usr
| `chwon -R pat bar`      | Change the owner of the directory bar and all of it's contents to the user pat
| `chmod 664 foo.txt`     | Change permissions of the foo.txt file allowing the user and the group to read and write and to the others only read
| `chmod +x bar.exec`     | Add execute permissions to the user, group and others (everyone) of the file bar.exec
| `chmod o-rw foo.txt`    | Remove read permissions to other of the file foo.txt
| `chmod -R ug+r bar`     | Add read and write permissions to the directory bar and all its contents to the user and the group
| `chmod 777 bar.txt`     | Give read, write and execute permissions to everyone. Should be avoided, use at your own risk.
