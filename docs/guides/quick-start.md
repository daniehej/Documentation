![logo](../assets/img/claaudia-logo.png "Title")
# CLAAUDIA Compute Cloud Strato - Quick start
Follow four steps to setup your first cloud instance and shelve, delete ot create an image when you're done. We will launch an instance with a m1.medium flavour(2 vCPUs, 4GB RAM and 40GB storage) based on the Ubuntu 18.04 image (Ubuntu 18.04 LTS + AAU specific setup) and connect to it with SSH (you will need to be located at AAU or use e.g. [VPN](https://www.en.its.aau.dk/instructions/VPN)).

| **Image**                                             |**Compute Resources**|**Accessible**     |
|-------------------------------------------------------|---------------------|-------------------|
| [**Ubuntu 18.04 image**](guides/image-guides/ubuntu)| m1.medium           | From the internet |

    
   1. [Login to the Cloud](#login-to-the-cloud) 
   2. [Setup Ubuntu cloud instance](#setup-an-ubuntu-cloud-instance)
   3. [Access the instance](#access-the-instance)
   4. [Delete, Shelve or to image](#delete-shelve-or-to-image)


## Login to the Cloud

To access the compute cloud go to **<https://strato-new.claaudia.aau.dk>** with your favorite browser. Ensure the *Authenticate Using* is set to **WAYF** and click **Connect**. You will be redirected to signon.aau.dk/... where you must login with your aau email and password, click **LOGIN** and now you should be send to the cloud dashboard.
![Alt Description](../assets/img/openstack/login.gif "Title")


## Setup an Ubuntu cloud instance

To create the ubuntu instance, common settings must be sat first. To access the ubuntu instance with SSH, port 22 must be open, furthermore an SSH Key-pair must be generated as login credentials.
 
### SSH rule

The compute cloud configures port access with **Security groups**. Each group can have a number of rules to permit access. Follow the steps to add port 22(for SSH) to the default security group.

1. Navigate to **network**
2. Click the sub menu **Security Groups**
3. Click **Manage Rules** on the default Security group
4. Add a rule
5. Choose SSH from dropdown menu.

![Alt Description](../assets/img/openstack/ssh_rule.gif "Title")

### Set SSH Key-pair

The compute cloud authenticates per default instances with a SSH keypair. Follow the steps to generate the key-pair we later will use to access the Ubuntu instance

1. Navigate to **Compute**
2. Click on the sub menu **Key Pairs**
3. Add new **Key Pair**
4. Fill out name
5. Save the public key locally
![Alt Description](../assets/img/openstack/Creat_Key_Pair.gif"Title")


### Launch Ubuntu instance

To launch the Ubuntu instance navigate to the "launch instance" menu using the webinterface.

1. Navigate to the project tab.
2. Click the **Compute** sub-tab.
3. Click on **Instances**.
4. Press **Launch instance** on the right side of the webpage.

![Alt Description](../assets/img/openstack/find_create_instance.gif "Title")

In the **launch instance** menu you can set settings for the instance. To launch your first ubuntu instance the following menu's settings must be applied.

- **source** Choose the Ubuntu 18.04 image. 
- **flavour** Apply the m1.medium compute resource for a small first instance.
- **networks** Select Campus Network 01. If you are interested in having an instance that is globally accessible for e.g. hosting a webservice, copying data from another university etc., and hence also at a higher security risk, then select AAU Public.
- **security groups** Ensure that the default security group we edited earlier is applied (should be as default). 
- **key-pair** Ensure that the key-pair created earlier is applied (should be as default).

![Alt Description](../assets/img/openstack/Create_instance.gif "Title")


### Access the instance

The default way of accessing is via SSH. The default username is **ubuntu** for the Ubuntu 18.04 image we chose. Use the key-pair we applied earlier in the guide as following to access the instance.

On mac/linux open the terminal on windows open PowerShell or Command Prompt
```bash
ssh ubuntu@10.92.0.zzz -i yourPersonalKey.pem
```


![Placeholder](../assets/img/openstack/ssh_instance.gif){ loading=lazy }



Remember, you need to be on the AAU network using e.g. VPN. If you get a "Connection timed out" error then the error is likely due to not being on the network - connect using your VPN client and try again.

**Congratulations!** You've successfully created your first cloud instance! If you don't plan to use this ubuntu 18_04 instance, please [delete it ](https://git.its.aau.dk/CLAAUDIA/docs_openstack/src/branch/master/OpenStack_guides.md#delete-an-instance)

On some platforms/tools you might receive an error, saying that Permissions are too open. This can be adjusted with either setting the right permissions for the file or move the file to a specific folder.

#### Mac/Linux
```bash
chmod 600 yourPersonalKey.pem
```
and then try to connect again.

#### Windows
Move the SSH key to the ".ssh" folder located in the home directory, for example 'C:\User\adf\.ssh' to set the right permissioins
and then try to connect again.

## Clean up instance

To delete an instance using the Horizon web interface you must be logged in. For an instance to be deleted, it must first be [Shut off](#shutdown-an-instance). Navigate to the **launch instance** menu using the horizon web interface.

1. Navigate to the project tab
2. Click the **Compute** sub-tab
3. Click on **Instances**
4. Mark the checkbox of the instance you wish to delete.
5. Press **Delete Instances** on the right side of the webpage.
6. press **Delete Instances** in the confirmation dialog.

![Alt Description](../assets/img/openstack/delete_instance.gif)

If you wish to keep the instance, instead of deleting it you can shelve it.

=== "Unordered list"

    * Sed sagittis eleifend rutrum
    * Donec vitae suscipit est
    * Nulla tempor lobortis orci

=== "Ordered list"

    1. Sed sagittis eleifend rutrum
    2. Donec vitae suscipit est
    3. Nulla tempor lobortis orci

