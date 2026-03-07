'sed' command usage
------------------
Remove Text between two patterns. <br>
For example, <br>
content of `filename.txt`:
```
2020-12-04 15:06:23,917 - [Mobile Service] [42.108.171.121]
```
result we want is,
```
2020-12-04 15:06:23 [Mobile Service] [42.108.171.121]
```
Command:
```
sed 's/,[^[+]*\[/\ [/' filename.txt 
```
Read complete line in 'for' loop with  spaces
```
IFS=$'\n'       # make newlines the only separator
for j in $(cat ./file_wget_med)    
do
    echo "$j"
done
# Note: IFS needs to be reset to default!
```
 
Add string after a certain string in the same line in a text file
```
sed "0,/members/{s/\bmembers */&$host, /}" printer.cfg
```

awk approach:
```
awk -v h=$host '!f && /members /{ $2=sprintf("%8s, %s",h,$2); f=1 }1' printer.cfg
```
Replace , with space:
```
sed -e 's/,/ /g' 
tr , \\n
```
Add line after some line:
```
sed -i 'client_script="purval" /a clientscript=hello'  {filename}
```
Remove empty space from file:
```
sed -i '/^[[:space:]]*s/d' {filename}
```
Add Word At End of The Line of Matched Pattern:
```
sed -i '/^all:/ s/$/ purval/' {file_name}
```
Append line at the end of the file using sed:
```
sed -i '' -e '$a\testing of adding new line at the end' file
```
Append line in specific location with using sed :
```
sed '1 a\appended line' your-file  (1 is line number)
```
append line into file which contains spaces before starting of line:
```
text="\ \ autohide=0" 
sed -i "6 i \ \ ${text}"  panel
```

Replace start of each line with Five Space :
```
sed -i -e 's/^/     /' panel
```

delete lines from searched pattern:
```
sed -i '/<context name="Root">/,+12d' filename
```
delete perticular line from from file ( with line numer ) :
```
sed -i '9d' {filename.txt}
```
command for kill zombie process:
```
sudo kill -HUP $(ps -A -ostat,ppid | grep -e '[zZ]'| awk '{ print $2 }')
```
Delete Lines Between Two line Numbers:
```
sed -i '4,10d' filename.txt
```
In case of delete from variable:
```
sed -i ''$first','$last'd' filename.txt
```
Delete Lines Between Two Searched pattern
```
sed '/PATTERN-1/,/PATTERN-2/d' input.txt
```
Remove specific character in every line :
```
sed -i 's/a/ /' filename 
```
Remove first character from every line :
```
sed -i 's/^./ /' filename
```
Remove last character from every line :
```
sed -i 's/$./ /' filename
```
Remove First and Lase character of every line:
```
sed 's/.//;s/.$//' filename
```
Remove First Character only,if it is a Specific character 
```
sed 's/^F//' file
```

Remove Last character only if it is specific character  :
```
sed 's/x$//' file
```
Remove First three characters from every line:

```
sed 's/...//' file
```
Remove First n characters from every line:
```
sed -r 's/.{4}//' file
```
Remove last n character of every line
```
sed -r 's/.{3}$//'
```
