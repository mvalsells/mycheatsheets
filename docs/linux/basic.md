# Linux basic commands
## Moving arround

#### Listing directories
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
| `pwd`       | Get absolute path to the current directory
