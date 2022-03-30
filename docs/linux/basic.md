# Linux basic commands

## Moving arround
### Listing directories
|Command      | Description
|-------------|--------------
| `ls`        | Show contents of the current directory
| `ls -l`     | Show contents of the current directory in a list, with permissions, owners, size and date/time
| `ls -lh`    | Same as `ls -l` but show size in human readable size (KB, MB, GB, etc.)
| `ls -a`     | Show hidden files
| `ls -S`     | Order by file size, largest first
| `ls -R`     | List recursively folders contents (I prefer `tree` over this one)
| `tree`      | List recursively folders contents with a nice view
| `tree -a`   | Also list hidden items
| `tree -L X` | Only list files and folder up to X levels
| `tree -d`   | Only list directories, ignore files
| `pwd`       | Get absolute path of the current directory

**The golden ones**

- `ls -lha` -> List all files (including hidden) in with human readable size

- `tree -L 3` -> Show files and folders of the current directory and up to 3 sub-directories

- `pwd` -> Get absolute path of the current directory
