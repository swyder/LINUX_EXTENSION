## Filesystem navigation

Command | Task
--------| --------------------------
pushd   | pushes a directory onto a stack and switches to it
popd    | removes the current directory from the stack and changes to the previous one
dirs    | list current stack
cd -    | change back to the previous directory


### Other useful commands

htop  
tree  

Compare sorted files FILE1 and FILE2 line by line (for Venn diagrams).  
comm  

### Following a log file as it grows
tail -f <some log file>  
less +F <some log file>   


### tricks using find

This command will find all the files accessed more than 10 days ago
```
find / -atime +10 -print  
```

The command below lists all files larger than 1Gb:
```
find / -size +1G 2>/dev/null | xargs du -h  
```

The line above doesn't work if file name contains spaces. Allowing for file names containing whitespaces:
```
find / -size +1G -print0 2>/dev/null | xargs -0 du -h
```

### shellcheck https://www.shellcheck.net/
Shellcheck finds bugs in bash scripts + recommends improvements


### Bash safe mode

```
set -euxo pipefail  
```

from https://vaneyckt.io/posts/safer_bash_scripts_with_set_euxo_pipefail/  
  
Often times developers go about writing bash scripts the same as writing code in a higher-level language. This is a big mistake as higher-level languages offer safeguards that are not present in bash scripts by default. For example, a Ruby script will throw an error when trying to read from an uninitialized variable, whereas 
a bash script won’t.


### brace expansion - filename or list generation

```
echo foo.{bar,baz,ree}
foo.bar foo.baz foo.ree
```

```
mv file{.txt,.bak} # mv file.txt file.bak
mv file{,.`date "+%F"} # mv file file.2011-11-27
```

---------------------------------------------------

## Shortcuts for Movement and commands (Emacs, work in terminal and also in Mac)

Shortcut | Explanation
---------| -----------
*Delete* |
ctrl-d | delete character
ctrl-k | delete from cursor till the end of the line (in some editors entire line) 
ctrl-u |to clear the current line
 |
*Move cursor* |
ctrl-a | jump to the beginning of the line
ctrl-e | jump to the end of the line
ctrl+arrow | jump to/between the first letter of words
 |
*Replacing arrow keys* |
ctrl-b | move cursor to the left 
ctrl-f | move cursor to the right 
ctrl-p | move cursor up 
ctrl-n | move cursor down
 |
*Other* |
ctrl+r | search history


### Some fun for the end

cowsay "URPP evolution"

### softlinks

ln -s ../scripts/Run_mapping.sh
ln -s MyLink ../scripts/Runmapping.sh 
unlink Run_mapping.sh   # Attention, unlink with a regular file deletes it
  
split  

