### Maximum number of history lines in memory
```
export HISTSIZE=50000
```
### Maximum number of history lines on disk
```
export HISTFILESIZE=50000
```

### Ignore duplicate lines
```
export HISTCONTROL=ignoredups:erasedups
```

### When the shell exits, append to the history file instead of overwriting it
```
shopt -s histappend
```

### After each command, append to the history file and reread it
```
export PROMPT_COMMAND="${PROMPT_COMMAND:+$PROMPT_COMMAND$'n'}history -a; history -c; history -r"
``` 
 
### Linux command history with date and time.
Run below commands.
```
echo 'export HISTTIMEFORMAT="%d/%m/%y %T "' >> ~/.bash_profile
source ~/.bash_profile
```
Or set it permenant, add below entry into `.bashrc`
```
export HISTTIMEFORMAT="%d/%m/%y %T "
```
