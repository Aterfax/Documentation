### How to connect a local synology drive to Petalibrary

#### Step 1: get on Synology as an admin user

_(In DiskStation Manager (DSM 4.2) you must use the admin user, in later versions you can be in the admin group.)_

admin2:~ % ssh damrauerstorage.colorado.edu -l admin
admin@damrauerstorage.colorado.edu's password:


BusyBox v1.16.1 (2013-04-16 20:13:10 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

damrauerstorage>



# run "ash" to break out of unrestricted shell

damrauerstorage> ash


BusyBox v1.16.1 (2013-04-16 20:13:10 CST) built-in shell (ash)
Enter 'help' for a list of built-in commands.

/volume1/homes/admin $


# verify that we can talk to the DTN ssh port

/volume1/homes/admin $ telnet dtn.rc.int.colorado.edu 22
SSH-2.0-OpenSSH_7.4


# generate an ssh key for connecting to petalibrary

/volume1/homes/admin $ ssh-keygen -t rsa -b 4096 -C 'damrauer synology'
Generating public/private rsa key pair.
Enter file in which to save the key (/var/services/homes/admin/.ssh/id_rsa):
Created directory '/var/services/homes/admin/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /var/services/homes/admin/.ssh/id_rsa.
Your public key has been saved in /var/services/homes/admin/.ssh/id_rsa.pub.
The key fingerprint is:
e9:52:0b:49:a3:1c:42:e7:96:04:1e:cb:de:28:a3:db damrauer synology
The key's randomart image is:


# add public key from synology to ~/.ssh/authorized_keys file on login.rc.coolorado.edu (must be account that will accept rsync jobs from synology)

cat .ssh/id_rsa.pub
...

# test connectivity from synology to dtn

/volume1/homes/admin $ sftp yila7873@dtn.rc.int.colorado.edu


# rsync command to move data

rsync --progress --inplace --protocol=29 -Havx "/volume1/Izzy/Izzy Lattke/Izzy Lattke/WINDOWS-1BE8BSU/Data/C/Users/Izzy Lattke/Google Drive/Teaching/." yila7873@dtn.rc.int.colorado.edu:/pl/active/Damrauer_Backup/synology_rsync_backup/.

# sample cron job

didn't work :(
