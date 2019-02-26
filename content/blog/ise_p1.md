+++
title = 'Setup a RAID system with encrypted LVM partitions on virtualbox'
date = '2019-02-26'
author = 'Yábir García'
tags = ['university', 'servers']
+++

## About 

This is the first practice of the subject `Server engineering`. I love
mounting servers like this and for this reason I want to write it up
in case someone else is interested.

## What we are doing

We are going to install Ubuntu Server 16.04 in a virtualbox using RAID
1 for the disk. We'll create some LVM partitions and encrypt
them. Lastly we'll setup a host-only connection and that's all.

## Step 1: Creating the virtual machine

Go to VirtualBox and follow this steps:

1. New
2. Choose whatever name, type: `Linux`, version: `Ubuntu 64 bits`.
3. We'll use 1GB of RAM so don't change the default value.
4. Create a virtual hard disk
5. VDI is fine
6. Dynamically allocated
7. 10GB is good now
8. Rigth click the new vm -> settings -> storage 

Click on `Empty` under `Controller: IDE` -> Next to optical drive in
the right side click on the disk and choose your iso file. Now we are
adding a second virtual disk, click on `Controller: SATA` -> Add hard
disk -> Create new disk and follow the default values. At least it
needs to have the same size as the first virtual disk since we're
doing a raid 1. You can close the settings window and start the machine.

## Step 2: Installing the system 

	Note: During the install process some keys that you'll use are:
	
	Enter -> Navigate in the menus and select things
	Space -> mark options


### Step 2.1: Setup the RAID

1. Choose your language
2. Install Ubuntu Server
3. Configure language and keyboard
4. Choose the name of the machine. It can be anything
5. Choose your username and password. Take note of this since you'll need them
6. When it asks about to cypher your home folder say `No`
7. Now we're going with the `Manual` option for the partitions
8. Click enter on both partitions and type `Yes` when it asks if you
   want to create a partition in the device.
9. Second option `Configure Software RAID`. Since we're doing a
   virtualization we're mounting a virtual RAID managed by the os.
10. Create a MD device and choose RAID1. Type 2 devices and 0 free
    (Default options in the first 2 menus). Now choose the two disks that we created.
11. Accept changes and choose Done.

### Step 2.2: Setup LVM

1. Choose the third option to configure LVM (`Configure the Local Volume Manager`)
2. Create group of volumes
3. Choose a name
4. Choose the MD that we created in the previous step. Now the
   distribution of numbers should be `0 1 1 0` in the main menu.
   
   ![](/images/ise/uno.png)
   
5. Create a logic volume and choose your group (there should only be one)
6. We need 4 volumes (`home`, `swap`, `system` and `boot`) you can use
   this names. I'm using 1GB for `home`, 1GB for `swap`, 250 MB for
   `boot` and the rest for `system`. Now the distribution should be `0 1 1 4`.
   Choose Done

### 2.3 Encrypt our partitions

In the main menu choose the third option (`Configure encrypted volumes`)

1. Create encrypted volumes
2. Choose `home`, `swap` and `system`. If you encrypt boot you won't be able to ... well boot the system.
3. Now you will see 3 times the same menu. We're configuring the
   different partitions, on the first name you can see the name of
   your LV.
   
   
| Name        | home  | swap      | system |
|-------------|-------|-----------|--------|
| Use as      | ext4  | swap area | ext4   |
| Mount point |`/home`|           | `/`    |
| Label       | home  |           | system |

4. Click on done
5. Now you're back to the main menu and click on the boot partition
   and choose `use as` ext4 and mount to `/boot`
6. Click again on create encrypted volumes and choose the 3 partitions
7. Click on Finish
8. It should ask 3 times (with their respective confirmation request)
   for a password, is a good practice to use different password but
   this is just a toy. Don't forget this passwords

All should be ready, you can check the changes to verify this. Go to
the last option (Finish and write to disk) and continue. The final
configurations should be similar to 

![](/images/ise/dos.png)
![](/images/ise/tres.png)

When it asks for a HTTP proxy just skip the step.
For the option of updated choose without automatic updates.
Standard system utilities is your friend.

### Step 2.4 Finishing installation

Lastly we need to configure the grub. Now we can only install it one
of the disk but since we're using a RAID 1 later we'll install it in
both disks.

Choose yes when the message promps in the screen and choose `/sda`.

When the installation is finished and your system is booting it'll ask
you for the different passwords that you introduced in step
2.2.8. Once ready sign in to your account.

Let's install the grun in sdb too. If you type `lsblk` you should see
both disks with your RIDE configuration. Type 

	sudo grub-install /dev/sdb

and it should prompt: `No errors found`

### Step 3: Configuring the Host-Only network

We can't proceed with the vm on so shutdown it now.
