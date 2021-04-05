### Copy data from a google drive location to a PetaLibrary allocation


<> # SKIP THIS SECTION, NOT NECESSARY
<> # create gogle drive service account

 <> - rclone docs are at https://rclone.org/drive/
 <> - visit https://console.developers.google.com/
<> - a project must exist. not sure what interface looks like if no projects exist, but the top sidebar should allow you o create a new project if one does not exist. select the project you want to use, or create a new project and select it.
<> - click 'credentials' on left sidebar
<> - click '+ create cendentials' on top
<> - select oauth client id'
<> - select 'desktop app'
<> - go to credentials, click credential name you just created, click 'download json' link at top.


#### download/install rclone and make sure you can run it

 - run 'rclone --version

#### configure google drive remote with rclone

* Type `rclone config` to create a new profile for Google Drive to PetaLibrary
* You will be prompted for whether to configure a new or existing profile
 * Type `n` for "new remote"
* You will be prompted to name the new profile
* Provide a name (e.g., `gdrive_johndoe_cu`)
* You will now be prompted for the type of storage to configure
** Select the number of the option for "Google Drive" (e.g., the number is "15" for _rclone_ v1.54.1)
* You will now be prompted for a client idy
** do not provide one (just hit enter)
* You will now be prompted for a client secret
** Do not provide one (just hit enter)
* You will now be prompted for scope that rclone should use when requesting access from drive
** For scope enter '1' for full drive access
* You will now be prompted for a root folder ID
** Do not provide one (just hit enter)
* You will now be prompted for a Service Account file.
** Do not provide one (just hit enter)
* You will now be prompted for "Edit Advanced Config File"?
** Select `no` (default)
* You will now be prompted for "Use Auto config"?  
** Choose "Y" for yes (default). Now _rclone_ will give you a URL to use to authenticate against. It may automatically open this URL in your browser.  If it does not, you can paste the URL into your browser if you are configuring on a local machine. If you are working on a remote system (e.g., if you are logged into your lab server from home), then from a terminal you can `ssh` from your computer to the system where you are configuring rclone:

   ```bash
   $ port=53682
   $ ssh -L ${port}:localhost:${port} <machine where rclone is running>
   ```
   
   ...now entering the url in your local browser should work.

* Once you are in your browser, you may be asked to authenticate to your Google account, and then you will be asked to allow Rclone to access the files in your `gdrive`. 
** Complete this step to grant access.  If successful you'll recieve a "success" message. 



# you should now have a gdrive remote. test it!

 - rclone ls gdrive_jesse_cus
 - if this step fails, your gdrive remote is not configured properly



# create petalibrary remote

 - rclone config
 - 'n' for new remote'
 - use name 'dtn_on_campus'
 - select ssh/sftp storage type
 - enter 'dtn.rc.int.colorado.edu' for host (dtn.rc.colorado.edu for off campus)
 - username can be blank (default to your username)
 - leve port blank
 - do not enter a password
 - do not enter key_pem
 - key_file should point to ssh private key that can auth against the dtns (meaning the public key is already on the dtns in ~/.ssh/authorized_keys)
 - should be able to selet defaults the everything else



# again, test remote

 - rclone ls dtn_on_campus:/pl/active/<allocation_name>



# test copying data around

 - rclone mkdir dtn_on_campus:/pl/active/rcops/jesse/rclone_test
 - rclone sync gdrive_jesse_cu:backups dtn_on_campus:/pl/active/rcops/jesse/rclone_test (backups is a directory in my gdrive account, this mirrors the contents of my gdrive backups directory to the jesse/rclone_test directory in the rcops allocation



# PROBLEMS

 - i don't know how to deauthorize rclone frmo connecting to the gdrive account
 - ssh pubkey documentation is weak, it assumes the host running rclone can connect to the dtns with a passphrase-free ssh key
 - documentation make no mention of potential dtn firewall issues
