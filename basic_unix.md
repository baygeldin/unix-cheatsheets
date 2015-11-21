# Basic Unix Cheatsheet

## Bash Shortcuts

`Ctrl + C` — cancel current terminal process   
`Ctrl + L` — clear the terminal  
`Shift + Page Up/Down` — pages of terminal (after a clearing)

`Ctrl + A` — start of a line  
`Ctrl + E` — end of a line

`Esc + b` — one word backwards  
`Esc + f` — one word forward

`Esc + c` — capitalize word  
`Esc + u` — upper-case word 
`Esc + l` — lower-case word

`Ctrl + U` — delete to the left  
`Ctrl + K` — delete to the right  
`Ctrl + W` — delete a word on the left
`Ctrl + Y` — revert deleting

`Ctrl + R` — search the history  
`Tab` — autocomplete

## Bash Useful Things

`!!` — last command (e.g. "sudo !!")

## Streams

`<command>` — execute a command and paste it inline (e.g. "echo \`pwd\`/test")  
`>` — redirect a stream to a file or another stream (1\> for stdin, 2\> for stderr)  
`>>` — redirect a stream to the end of a file (e.g. "sort << END" helps to pipe stdin to sort)  
`|` — pipes result of a command to the next command

`xargs <command> <arguments>` — execute a command with given arguments  
`xargs -0 ..` — set 0 as delimiter (e.g. "find . -name "\*.sh" -print0 | xargs -0 rm -rf")  
`xargs -n <number> ..` — splits arguments by n and execute a command  
`xargs sh -c '<command>; <command>;' ..` — xargs with multiple commands

## File System

`ls -a` — list ALL files  
`ls -lh` — detailed human-readable list (with rights)  
`ls *.jpg` — jpeg only files  
`ls -1` — list representation (e.g. ls -1 | sort)

`mkdir myfolder/another/ ..` — create a folder

`pwd` — print a working directory

`du -h` — disk usage of folders

`touch <file>` — create/update a file

`mv <file/folder> <folder>` — move everything to anywhere  
`mv <file> <file>` — rename a file

`cp <file> <file>` — copy and rename a file  
`cp -R <folder> <folder>` — copy and rename a folder (recursive)  
`cp *.txt <folder>` — copy all txt's to a folder

`rm <file>` — delete a file  
`rm -rf <folder>` — force deleting a folder

`ln <file> <file>` — physical link (very low-level concept)  
`ln -s <file> <file>` — just a symbolic link


## File Processing

`cat <file>` — content of a single file  
`head -n <number> <file>` — just head of a single file  
`tail -n <number> <file>` — just tail of a single file

`less <file>` — cat on steroids (commands: <number>G, g, q, arrows, page up/down)

`sort <file>` — sort lines of file alphabetically  
`sort -o <file> <file>` — put results to a file  
`sort -r <file>` — sort in reverse  
`sort -R <file>` — sort randomly  
`sort -n <file>` — sort numbers

`wc <file>` — word count  
`wc -l <file>` — lines count  
`wc -n <file>` — char count  
`wc -c <file>` — bytes size

`diff -rc <file/folder> <file/folder>` — diffrences with context (recursive)

`patch -i <file>.diff <file>` — apply diff to a file (need full path to .diff file)  
`patch <file> < <file>.diff` — same as above, but don't need full path  
`patch -d <folder> < <file>.diff` — apply diff to a whole folder  
`patch -p<number> ..` — cusomize diff to it's location (typically you need -p1)


## Search in files

`find <optional folder> -name "<pattern>"` — list files which filenames suits the pattern  
`find .. -type d` — only files (or "-type d" for directories)  
`find .. -atime +-<time>` — find from last access  
`find .. -size +-<size><M/K/G>` — find files bigger/smaller than  
`find .. -print0` — set null char as a delimiter

`grep <text> <file>` — list of lines with given text in a single file  
`grep -r <text> <folder>` — list of lines with given text in all folder (with filenames)  
`grep -E <regex> <file>` — same, but with regex (e.g. "grep -r -E ^Alex .")


## Time & Sheduling

`date "+%H:%M:%S of %Y"` — format the date and print it

`at <time>` — shedule tasks (Ctrl + D to close input)  
`at now +<number> <minutes/hours/days/weeks> ..` — shedule from now  
`atq` — list of tasks with id's  
`atrm <id>` — delete a task

`crontab -e` — modify the crontab  
`crontab -l` — list the crontab  
`crontab -r` — delete the crontab

`sleep <seconds>` — puts thread to sleep for a number of seconds


## Process Management

`&` — execute in the background (e.g. "cp large_file.txt folder/ &")  
`jobs` — what is running in the background  
`fg <id>` — put a background process to the foreground

`w` — who is logged on and what they are doing

`ps` — static process list  
`ps -ef` — all static process list  
`ps -uf` — only from current user  
`ps -ejF` — show process hierarchy

`top` — show dynamic process list (q to quit, k to kill a process)

`kill <PID>` — kill a process

`sudo halt` — shut down the computer  
`sudo reboot` — reboot the computer


## Rights

`chown <username>:<optional group> <filename>` — change the owner of a file  
`chown -R ..` — change the owner of a folder (recursive)

`chmod <number> <file>` — change mode manually  
`chmod <u/g/o>+-<r/w/x> <file>` — add/remove rights  
`chmod -R ..` — change mode of a folder (recursive)


## Users

`sudo adduser <username>` — create a new user  
`sudo paawd <username>` — change user's password  
`sudo deluser <username>` — delete a user

`addgroup <group>` — create a new user group  
`delgroup <group>` — delete a user group

`usermod -g <group> <username>` — add user to a group


## Compress data

`gzip <file>` — zip a file and delete it (just a compression algorithm, ONLY for files)  
`gunzip <file>.gz` — unzip a file and delete an archive  
`gunzip -c <file>.gz > <file>` — unzip a file without deleting archive

`tar cvf <file>.tar <file/folder>` — create an archive  
`tar xvf <file>.tar` — extract an archive to a folder  
`tar tf <file>.tar` — view a content of an archive  
`tar xzvf <file>.tgz` = `gunzip -c <file>.tar.gz | tar xv` — extract a tgz archive  
`tar xIvf <file>.tar.bz2` — extract a tar.bz2 archive
