### Vim

Run vim tutor in any machine with vim installed
```bash
> vimtutor
```

### Tmux
```bash
tmux                    starts a session
tmux ls                 lists all sessions
tmux attach -t <0>      attaches to session # <0>

hierarchy of tmux: session >> window >> panes


Ctrl-B + %          : create a new pane; split horizontally
Ctrl-B + "          : create a new pane; split vertically
Ctrl-B + <up arrow> : focus on pane above
Ctrl-B + z          : toggle fullscreen on a pane

Ctrl-B + c          : create a new window [lists on status bar as 0:bash; 1:bash]
Ctrl-B + <window number>     : focus on window # <window number>


Ctrl-B + d          : detach from current session
tmux attach -t <session number> : attach to session # <session number>
```
### Putty settings
* window
  - appearance
    - font: Ubuntu Mono, 10-point
    - font quality: antialiased
  - translation
    - remote character set: UTF-8
  - colours
    - Indicate bolded text by changing () the font, (x) the colour, () both

* connection
  - data
    - auto-login username: control
    - terminal-type string: xterm-color

### Get Apt history
```bash
# check apt / apt-get install history
cat /var/log/apt/history.log
cat /var/log/dpkg.log
```

### Find IP from MAC
To find IP address in the network using MAC address
```bash
sudo arp-scan -I eth0 10.80.9.0/24 | grep -i "0c:c4:7a"
```

### Users
Add new user
```bash
useradd -m -U $USER         # -m = creates home dir, # -U creates a group with the same name as user
useradd -m -U control       # adds user control, with home directory
```

Change password of user `$USER`
```bash
passwd $USER
passwd control  # to set a password for user "control"
```
Add user `$USER` to group `$GROUP`
```bash
usermod -aG $GROUP $USER
usermod -aG docker control    #for docker access
# Logging out and logging back in is required because the group 
# change will not have an effect unless your session is closed.
usermod -aG sudo control      # -a appends, -G <groupname> <username>, 
                              # this adds user "control" to group "sudo"
```


## Shell

### Change default shell of `$USER`
```bash
usermod --shell /bin/bash $USER
usermod -s /bin/bash control
```

### List available shells
```bash
cat /etc/shells
```


### Who is logged in
```bash
# check logged-in shells (ssh or not)
who
who -a
```
### Clear Bash History and Exit

```bash
# clear bash history
cat /dev/null > ~/.bash_history && history -c && exit
```

### Console output to file
To write the output of a command to a file, there are basically 10 commonly used ways.

Overview:
Please note that the n.e. in the syntax column means "not existing".
There is a way, but it's too complicated to fit into the column. You can find a helpful link in the List section about it.
```
          || visible in terminal ||   visible in file   || existing
  Syntax  ||  StdOut  |  StdErr  ||  StdOut  |  StdErr  ||   file   
==========++==========+==========++==========+==========++===========
    >     ||    no    |   yes    ||   yes    |    no    || overwrite
    >>    ||    no    |   yes    ||   yes    |    no    ||  append
          ||          |          ||          |          ||
   2>     ||   yes    |    no    ||    no    |   yes    || overwrite
   2>>    ||   yes    |    no    ||    no    |   yes    ||  append
          ||          |          ||          |          ||
   &>     ||    no    |    no    ||   yes    |   yes    || overwrite
   &>>    ||    no    |    no    ||   yes    |   yes    ||  append
          ||          |          ||          |          ||
 | tee    ||   yes    |   yes    ||   yes    |    no    || overwrite
 | tee -a ||   yes    |   yes    ||   yes    |    no    ||  append
          ||          |          ||          |          ||
 n.e. (*) ||   yes    |   yes    ||    no    |   yes    || overwrite
 n.e. (*) ||   yes    |   yes    ||    no    |   yes    ||  append
          ||          |          ||          |          ||
|& tee    ||   yes    |   yes    ||   yes    |   yes    || overwrite
|& tee -a ||   yes    |   yes    ||   yes    |   yes    ||  append
```
List:

```bash
command > output.txt
```
The standard output stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, it gets overwritten.
```bash
command >> output.txt
```
The standard output stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.
```bash
command 2> output.txt
```
The standard error stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, it gets overwritten.
```bash
command 2>> output.txt
```
The standard error stream will be redirected to the file only, it will not be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.
```bash
command &> output.txt
```
Both the standard output and standard error stream will be redirected to the file only, nothing will be visible in the terminal. If the file already exists, it gets overwritten.
```bash
command &>> output.txt
```
Both the standard output and standard error stream will be redirected to the file only, nothing will be visible in the terminal. If the file already exists, the new data will get appended to the end of the file..
```bash
command | tee output.txt
```
The standard output stream will be copied to the file, it will still be visible in the terminal. If the file already exists, it gets overwritten.
```bash
command | tee -a output.txt
```
The standard output stream will be copied to the file, it will still be visible in the terminal. If the file already exists, the new data will get appended to the end of the file.

---

Bash has no shorthand syntax that allows piping only StdErr to a second command, which would be needed here in combination with tee again to complete the table. If you really need something like that, please look at "How to pipe stderr, and not stdout?" on Stack Overflow for some ways how this can be done e.g. by swapping streams or using process substitution.
```bash
command |& tee output.txt
```
Both the standard output and standard error streams will be copied to the file while still being visible in the terminal. If the file already exists, it gets overwritten.
```bash
command |& tee -a output.txt
```
Both the standard output and standard error streams will be copied to the file while still being visible in the terminal. If the file already exists, the new data will get appended to the end of the file.
