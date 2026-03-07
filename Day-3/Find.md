`find` command:
---------------
List all files from all subfolders in linux:
```
find . -type f -printf "%T@ %p\n" | sort -nr | cut -d\  -f2-
```
List all subfolders:
```
find /dir -type d
```

### Error : "-bash: /usr/bin/find: Argument list too long" error while listing and removing files.

### Solution:

First navigate to folder where you want to delete files, don’t use `ls`, `ls -la` as it can take lot of time before it will show result,<br>
So, just make sure your in correct directory before running this command as it will delete all files in that directory. <br>

```
find . -type f -exec rm -v {} \;
```

Command will find all files one by one and will delete it. <br>

If you want to delete only specific files for example only `.jpg` files.
```
find . -maxdepth 1 -name "*.jpg" -print0 | xargs -0 rm
```
It will search files with `.jpg` extension in current directory and will delete it one by one.
 
