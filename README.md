### University of Zurich
### URPP Evolution in Action
![URPP logo](Logo_URPP_kl2.png)

Stefan Wyder & Carla Bello

stefan.wyder@uzh.ch  
carla.bello@ieu.uzh.ch


## Linux extension


Topic             | Person 
----------------- | --------------------------
Servers (ssh, scp, screen, nohup, tmux etc.) | SW
File transfer protocols (ftp) and wget | SW
[Backup (tar, rsync, rsnapshot)](Backup.md) | SW
Aliases | CB
Software installation and troubleshooting | CB
Parallel GNU | CB
Sending jobs to the cluster | CB
Exercises | CB&SW


## Interacting with servers / cluster / remote computers

## connecting to a remote host, transferring files

**Command** | **Task**
------------|----------
ssh -X *user*@*hostname* | Connect to server
scp \<what\> \<towhere\> | Transfer file from/to server
sftp *user*@*hostname* | Transfer file from/to server

ssh ("secure shell") is a secure protocol for remote login and also for executing commands in a remote machine. To connect to a remote machine using ssh, simply type:
```
ssh -X username@hostname
```
or with the IP address
```
ssh -X username@130.60.201.40
```
inside your local computer shell. You will be asked for the password and after you successfully login, 
you can work with the remote machine in the same way as you work on your local machine. (You are dropped at your home directory). But ssh can’t transfer files, for that you can use another program called scp or sftp. Disconnect before you go on to the next step (Ctrl+d).  
  
For Windows often used ssh clients are PuTTY and MobaXterm.  

For easier login and increased security use [Public key authentication](https://help.ubuntu.com/community/SSH/OpenSSH/Keys).  
  
  
`scp` is for secure file copying.
```
scp -p -r *.py username@hostname:~/mnt/mnemo2/scripts/
```
copies all python scripts of the current directory (`*.py`) including all subfolders (`-r`) to a specific directory (~/mnt/mnemo2/scripts/) on the server.
The `-p` preserves the modification time so that the copied files have the same timestamp than the original files.

## Running jobs in the background

By default, the terminal is blocked until the job is finished.

ampersand `&` at the end of your command -- it runs in the background and notifies you when its done, allowing you to work on something else:
```
cp * /tmp/ &
```

We can put unintentionally long jobs to the background:  

1. `ctrl-z` suspends the running program
2. `bg` runs suspended program to the background  
  

## Exit a running shell session without stopping running jobs

By default, all running jobs are stopped when we exit a shell session (log out or when the connection is interrupted). This is can be a problem for long jobs.
```
nohup long_running command &
```
will run it and save the output to `nohup.log`.  
  

Alternatively, runs the long_running command and saves the output to log.bwa:  
```
nohup long_running command 2>log.bwa &
```

### advanced

- You can check jobs using `jobs` and also bring them back to foreground using `fg`.  
- Another way to detach a running process is to use `disown`.


### screen and tmux

allow to

- Use multiple shell windows from a single SSH session.  
- Keep a shell active even through network disruptions.
- Disconnect and re-connect to a shell sessions from multiple locations.
- Run a long running process without maintaining an active shell session.


## screen command

### Installation on Ubuntu

```
sudo apt-get install screen
```

### Usage

(source of this slide: Genetic Diversity Center GDC)  

The command screen starts a new virtual terminal. This is very handy when you want to run a job that needs a long time to finish. Start the virtual terminal with:  
```
screen -S myname
```
Now you are inside the new screen. Start your job and detach from the screen with:  
```
ctrl-a d (press and hold control, type A, release A and control, type D)
```
Now you are back in your normal terminal.  
Later, when you want to go back to your screen, enter the command:  
```
screen -r ID.myname
```
Now you are back inside the screen. You can reconnect from anywhere (office, home), as long as you have access to a ssh terminal.  
  
If that screen hasn't detached, you can reconnect with the command:
```
screen -rd ID.myname
```
  
If you need to know the names of all your screens:
```
screen -ls
```
When you are inside a screen and you want to end it (not just detach), type exit and return.  
Careful: This will kill all running jobs in the screen!  

Find more information a detailed example [here](https://www.rackaid.com/blog/linux-screen-tutorial-and-how-to/)


## Running screen directly from SSH

command                       | explanation
----------------------------- | --------------------
ssh -t username@server screen | login and run screen on server
ssh -t username@server screen -r | login and directly resume session (if there is only one)


It is also possible to create and use multiple windows within the same SSH session. see this [tutorial](https://hostpresto.com/community/tutorials/how-to-use-screen-on-linux/).



### Exercises

1. Connect to your server of choice (e.g. ScienceCloud) using ssh
2. Let's check whether we have running screen or not: `screen -ls`
3. No screen found. Let's create a new screen: `screen`. Press enter to close the welcome screen.
4. 


## [tmux](https://github.com/tmux/tmux/wiki) https://github.com/tmux/tmux/wiki

Many people think of tmux, a so-called terminal multiplexer, as an easier-to-use and a little more powerful alternative to Screen.
  
It lets you switch easily between several programs in one terminal, detach them (they keep running in the background) and reattach them to a different terminal. It also provides vertical and horizontal window split support.  
  
- a tmux [crash course](https://robots.thoughtbot.com/a-tmux-crash-course)
- another [guide](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)
