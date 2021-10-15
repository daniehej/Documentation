![Alt Description](../img/logo.png?raw=true "Title")


# Move to strato
If you've been a user on the compute cloud pilot and want to move to strato you have three options.

1. Simply deploy the instance you want on strato and start using it.
2. Transfer your data from your old instance to the new instance.
3. Create an image of your instance and upload that image to strato.


## Move data
If you have data, that you can't easily recreate you can move your date with [SCP](https://www.howtogeek.com/66776/how-to-remotely-copy-files-over-ssh-without-entering-your-password/), [SFTP](https://linuxconfig.org/how-to-setup-sftp-server-on-ubuntu-18-04-bionic-beaver-with-vsftpd), [Rsync](https://www.digitalocean.com/community/tutorials/how-to-copy-files-with-rsync-over-ssh) etc. to your new instance on strato.


### SCP

```
scp -i <private_key> -r  <From_Folder_Path> ubuntu@<To_Address>:<To_Folder_path>
```
Example:
```
scp -i key.pem -r  /home/ubuntu ubuntu@130.226.98.198:/home/ubuntu
```
### Rsync

```
rsync -Pav -e "ssh -i <private_key>" <From_Folder_Path> ubuntu@<To_Address>:<To_Folder_path>
```
example:

```
rsync -Pav -e "ssh -i key.pem" /home/ubuntu ubuntu@130.226.98.198:/home/ubuntu
```
## Move image
If you have a big and complicated setup you can make an image of your pilot instance, download that image and upload it to strato. Then you'll continue where you were. (Be aweare that a new public IP on strato would be different from your cloud instance)



### Step 1 Detach your volume
Before creating an image of your volume so that you can download it, you need to detach the volume from your instance.

First, let’s stop your instance where the volume is attached too. You can view the instance name of your instance using the following command:

```
openstack server list
```

If the instance is still running you can Shut down your instance using the following command:

```
openstack server stop <instance_name>
```
Next, we need to detach the volume from the instance it is mounted on. First, let’s check the volume ID

```
$ openstack volume list
```
You can detach the volume using the following command:

```
$ nova volume-detach <instance_name> <volume_id>
```
If the volume is detached, you can go to step 2 otherwise follow Step 1.1

### STEP 1.1

To create an image of an attached volume, we are first going to create a snapshot from our bootable volume. After that, we are going to create a new volume from that snapshot. This way we have a volume that is unattached so that we can create an image of that volume.

First, let’s create a snapshot of the (bootable)volume

```
openstack volume snapshot create --volume <volume_name> --force <snapshot_name>
```
We can check the status here:
```
openstack volume snapshot list
```

Now let’s create a new volume from our freshly made snapshot. Please note that the volume size should match the snapshot size.

```
$ openstack volume create --snapshot <snapshot-name-or-id> --size <size> <new-volume-name>
```
### STEP 2 Create an image of your volume
You can not download a volume in OpenStack, so we first have to create an image of your volume so that the image can be downloaded.

Get your image ID using the following command:

```
openstack volume list
```
Create an image of your volume and give it a proper name using the following command. Be awear that using --disk-format qcow2 will reduce the size of the image.

```
openstack image create --volume <volume_name> <your_image_name> --disk-format qcow2
```
You can check the status of your image using the following command:

```
openstack image list
```
### STEP 3 Download your image
You can now download your newly created image using the following command

```
$ openstack image save --file snapshot.raw <your_image_id>
```
### STEP 4 Uploading your image to the new OpenStack platform
Make sure you are logged in to the correct OpenStack platform with your CLI tools

```
source your_openrc.sh
```
Upload your snapshot that you’ve just downloaded using the following command:

```
openstack image create --container-format bare --disk-format qcow2 --file <image_file_name> <image_name>
```
### STEP 5 Creating a volume from an image
You need the image ID of the image that you have uploaded to the new platform. You can obtain this using the following command:

openstack image list
Now we need to create a volume from the image that we have uploaded. You can do that using the following command:

```
openstack volume create --image <image_id> --size <size_in_gb> <name_your_volume>
```
You can check the status of your volume

openstack volume list
Start your instance with your bootable volume

```
openstack server create --volume <volume_name> --flavor <flavor_name> <instance_name>
```

