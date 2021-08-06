# Backup

- https://ubuntu.com/server/docs/backups-archive-rotation
- https://ubuntu.com/server/docs/backups-shell-scripts
- https://www.redhat.com/sysadmin/backup-dirs
- https://www.gnu.org/software/tar/manual/html_node/exclude.html#exclude
- https://stackoverflow.com/questions/984204/shell-command-to-tar-directory-excluding-certain-files-folders

Create a new backup file:

```
sudo nano /usr/local/bin/backup.sh
```

Insert this content to it:

```
#!/bin/bash
####################################
#
# Backup to directory script with
# grandfather-father-son rotation.
# Based on: https://ubuntu.com/server/docs/backups-archive-rotation
#
####################################

# What to backup. 
backup_files="/etc /home /root /var/lib/docker /var/log /usr/local/bin /usr/local/sbin /opt"

# Where to backup to.
dest="/mnt/backup"

# Setup variables for the archive filename.
day=$(date +%A)
hostname=$(hostname -s)

# Find which week of the month 1-4 it is.
day_num=$(date +%-d)
if (( $day_num <= 7 )); then
        week_file="$hostname-week1.tgz"
elif (( $day_num > 7 && $day_num <= 14 )); then
        week_file="$hostname-week2.tgz"
elif (( $day_num > 14 && $day_num <= 21 )); then
        week_file="$hostname-week3.tgz"
elif (( $day_num > 21 && $day_num < 32 )); then
        week_file="$hostname-week4.tgz"
fi

# Find if the Month is odd or even.
month_num=$(date +%m)
month=$(expr $month_num % 2)
if [ $month -eq 0 ]; then
        month_file="$hostname-month2.tgz"
else
        month_file="$hostname-month1.tgz"
fi

# Create archive filename.
if [ $day_num == 1 ]; then
    archive_file=$month_file
elif [ $day != "Saturday" ]; then
        archive_file="$hostname-$day.tgz"
else 
    archive_file=$week_file
fi

# Print start status message.
echo "Backing up $backup_files to $dest/$archive_file"
date
echo

# Backup the files using tar.
tar czf $dest/$archive_file $backup_files
#tar czf $dest/$archive_file --exclude=/var/cache --exclude=/var/tmp --exclude=/var/log $backup_files

# Print end status message.
echo
echo "Backup finished"
date

# Long listing of files in $dest to check file sizes.
ls -lh $dest/
```

Make it executable:

```
sudo chmod u+x /usr/local/bin/backup.sh
```

Create the directory for storing backups. 

```
sudo mkdir /mnt/backup
```

NOTE: This directory should be connected to a network drive (NFS, SAMBA etc.), so the backups are stored
outside of the backed up server! 

NOTE2: If a network drive is not used, make a copy/backup of this directory using ssh regularly.

You can test the backup script:

```
sudo /usr/local/bin/backup.sh
```

Add the backup script to the CRON scheduled tasks:

```
sudo crontab -e
```

Add this line there:

```
0 0 * * * bash /usr/local/bin/backup.sh
```

To see a listing of the archive contents. From a terminal prompt type:

```
tar -tzvf /mnt/backup/host-Monday.tgz
```

To restore a file from the archive to a different directory enter:

```
tar -xzvf /mnt/backup/host-Monday.tgz -C /tmp etc/hosts
```

Also, notice the leading “/” is left off the path of the file to restore.

To restore all files in the archive enter the following:

```
  cd /
  sudo tar -xzvf /mnt/backup/host-Monday.tgz
```

Note: This will overwrite the files currently on the file system.
