### University of Zurich
### URPP Evolution in Action
![URPP logo](Logo_URPP_kl2.png)

Stefan Wyder


### Backup

- hard drives crashes are frequent
- you could accidentally rename, move, reformat or overwrite files, or delete rows
- Regular backups can save you a lot of work and nerves
- keep multiple copies at different locations (also off-site)
- test your backups by restoring them!
- make sure you know how your server is backuped
- ideally different strategies for raw data & scripts >> bulky intermediate files  

## Make files read-only

Read-only files can not accidently be replaced or removed.
```
chmod 0444 Example_File  
```  

## rsync

https://help.ubuntu.com/community/rsync

- It can copy locally, to/from another host over any remote shell
- efficient because it only transfers files which are different between the source and destination directories
- installed by default
- Graphical user interface for rsync: [Grync](https://help.ubuntu.com/community/rsync#Grsync)
- usually started manually - can be automated, see Rsync Daemon in https://help.ubuntu.com/community/rsync

### Local backup

```
sudo rsync -azv /home/path/folder1/ /home/path/folder2
```

- `-a` archive mode (same as -rlptgoD)
  - Descend recursively into all directories (-r)
  - **copy symlinks as symlinks (-l)**
  - preserve file permissions (-p)
  - preserve modification times (-t)
  - preserve groups (-g)
  - preserve file ownership (-o)
  - preserve devices as devices (-D) 
- `-z` compresses the data
- `-v` verbosity of the reporting process  
  
Folder1 is the original folder, and 2 is the new folder, or existing one to be brought in sync with the first.  
Note that there is a trailing `/` after Folder1:  so that only the contents, rather than whole folder, would be moved into the second. 

### Backup over a network

```
sudo rsync --dry-run --delete -azv -e ssh /home/path/folder1/ remoteuser@remotehost.remotedomain:/home/path/folder2
```

- `--dry-run` Dry-run. It will just write a log of what it would do to the screen. Once you've made sure everything will work as you expect, you have to remove this option.
- `--delete` deletes files that don't exist on the system being backed up.(Optional)
- `-e` specifies remote shell to use 

## Backup using tar

`tar` is an incredibly useful tool. Importantly, it keeps the directory structure of your folders, file permissions, etc. You can also compress your files at the same time.  
  
This line will create a new tar file example.tar.gz from the folder example_analysis and its subfolders
```
tar -czvf example.tar.gz ./example_analysis
```

- `-c` creates new archive
- `-z` uses gzip to compress files
- `-v` verbosely list files being archived
- `-f` file name


  
  
Here we backup only specific files and/or file types. For using tar for a system backup see [here](https://help.ubuntu.com/community/BackupYourSystem/TAR)

```
datestr=`date '+%Y%m%d%H%M'`
cd /mnt/HD2/BACKUP/
find ~ /mnt/HD* -iname "Readme*.txt" | grep -v "/home/wyder/Downloads" > files2Backup
tar -cvz -T files2Backup  -f "Backup_Readmes_"${datestr}".tar.gz"
```

then copy the tar.gz file onto your NAS

## Updating tar

A nice feature of tar is that you can update tar files that you already created (i.e., you can create monthly addendums to your tar files).  
  
```
gunzip example.tar.gz
tar -uvf example.tar ./new_analysis
gzip example.tar
```


## Incremental Backup

`tar --newer` only includes recently modified files, here at most 8 days ago. We could also write "1 week ago" or "2 months ago".
```
datestr=`date '+%Y%m%d%H%M'`
cd /mnt/HD2/BACKUP/
tar -czvlf incr_backup-$datestr.tar.gz -X incr_backup.exclude --newer-mtime="8 days ago" ~
```

### The file incr_backup.exclude contains file types that are very large
```
more incr_backup.exclude
*.bam
*.bam.bai
*.sam
*.sai
*.sra
*.fastq
*.fastq.gz
```


## Backup of scripts

```
find ~ /mnt/HD* -regextype posix-egrep -regex ".*\.(py|pl|R|r|sh)$" | grep -v "/home/wyder/APPL" | grep -v "/home/wyder/APPL_FEDORA17" |grep -v "/home/wyder/Downloads/" | grep -v "/home/wyder/R/" | grep -v "/home/wyder/lib/" | grep -v "/home/wyder/bin/" > scripts2Backup
tar cvz -T scripts2Backup -f "Backup_scripts_"${datestr}".tar.gz"
```

## Backup of Readme files

```
find ~ /mnt/HD*  -iname "Readme*.txt" | grep -v "/home/wyder/APPL" | grep -v "/home/wyder/Downloads" > Readmes2Backup
tar cvz -T Readmes2Backup -f "Backup_Readmes_"${datestr}".tar.gz"
```

## Extracting tar Files into one folder without creating subfolder structure 

```
zmore Bck_R_Files.tar.gz | pax -v -r -s '/.*\///p'
```

## Ubuntu Backups

- preinstalled
- intuitive graphical user interface




## [rsnapshot](https://rsnapshot.org/) https://rsnapshot.org/

- based on rsync
- allows to keep a certain number of backups


## [Syncthing](https://syncthing.net/) https://syncthing.net/

- for syncing data between your devices
- available for lots of platforms including mobile phones
- easy to set up and run
- no central server involved

[Getting started](https://docs.syncthing.net/intro/getting-started.html#getting-started)

## Links

- [https://help.ubuntu.com/community/BackupYourSystem](https://help.ubuntu.com/community/BackupYourSystem)
