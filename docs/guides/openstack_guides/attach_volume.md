# Attaching volumes for additional storage

If you need additional storage, you can attach a volume to a running instance, make a filesystem and then mount the new filesystem on the attached volume.

Be aware that each has by default 10TB storage.

Guide:
1. Go to Volumes/Volumes and then click create volume. Give it a name to make it easier to locate and a size. Make sure its of a volume, not an image or snapshot.
2. Find the instance under Compute/Instances and under actions for the relevant instance select "Attach Volume". Select the volume you just have created.
3. You should see a popup with the name of new device, e.g. /dev/vdb /dev/vdc etc. You can also click on the instance name to to get info on e.g. the attached volumes and their naming.
4. SSH to the instance and create a new filesystem for the newly attached device/volume with e.g. (this makes existing data inaccessible. Dont do this step if theres data you need to keep)
```bash
$ sudo mkfs.ext4  /dev/vdb
```
5. Make a mount point in e.g. the home dir:
```bash
mkdir vol1
```
6. Mount
```bash
﻿sudo mount /dev/vdb ~/vol1
```
7. Verify the storage exists:
```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            7.9G     0  7.9G   0% /dev
﻿................................................
/dev/vdb        976M  2.6M  907M   1% /home/ubuntu/vol1
```
8. Update owner from root to ubuntu user
```bash
sudo chown ubuntu ~/vol1
```
