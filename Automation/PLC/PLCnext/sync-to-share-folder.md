# Syncing files from PLCnext

A common need is to send files to an external machine. [Rsync](https://www.digitalocean.com/community/tutorials/how-to-use-rsync-to-sync-local-and-remote-directories) is a powerful tool that can be used for this. It is preinstalled on PLCnext Linux. There are certainly other options but this has the advatange that no additional dependencies are required for this to run.

In this method, a simple script is created to sync files from the PLCnext (`~/share/` in this example) to a remote directory. The remote directory used is a Windows Share Folder. (Permissions to change the folder are required.)

```
#!/bin/bash

# Variables
REMOTE_SHARE="//Server/Path/To/Share"
USERNAME="myRemoteUser"
PASSWORD="myRemotePassword"
LOCAL_MNT="/opt/plcnext/mnt"

# Create local mount point
sudo mkdir -p $LOCAL_MNT

# Mount the remote share
sudo mount -t cifs $REMOTE_SHARE $LOCAL_MNT -o username=$USERNAME,password=$PASSWORD,uid=$(id -u),gid=$(id -g)

# Sync FROM local folder TO remote share
rsync -avz /opt/plcnext/share/ $LOCAL_MNT/

# Unmount the remote share
sudo umount $LOCAL_MNT
```

_To mount in PLCnext, root user is required to run._
