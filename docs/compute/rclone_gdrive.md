### copy data from google drive to a PL allocation



# SKIP THIS SECTION, NOT NECESSARY
# create gogle drive service account

 - rclone docs are at https://rclone.org/drive/
 - visit https://console.developers.google.com/
 - a project must exist. not sure what interface looks like if no projects exist, but the top sidebar should allow you o create a new project if one does not exist. select the project you want to use, or create a new project and select it.
 - click 'credentials' on left sidebar
 - click '+ create cendentials' on top
 - select oauth client id'
 - select 'desktop app'
 - go to credentials, click credential name you just created, click 'download json' link at top.



# download/install rclone and make sure you can run it

 - run 'rclone --version



# configure google drive remote with rclone

 - rclone config
 - 'n' for new remote
 - provide name (i used gdrive_jesse_cu)
 - select appropriate number for google drive (15 with rclone 1.54.1)
 - do not provide any data for client_id or client_secret
 - for scope enter '1' for full drive access
 - leave root_folder_id blank
 - leave service_account_file blank
 - rclone will give you a URL to use to authenticate against. the url will look like:

   http://127.0.0.1:53682/

   open this url in your browser. if this is on a remote system, you can use an ssh from your computer to the system where you are configuring rclone:

   port=53682
   ssh -L ${port}:localhost:${port} <machine where rclone is running>

   now entering the url in your local browser should work.

 - select 'no' for configuring as a shared team drive



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
