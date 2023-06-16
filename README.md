# Boot-Google-Coral-from-SD-card
### Below are the steps documented to boot the google coral board from SD card permanently.

- Insert the SD card and get its ID
- `sudo fdisk -l`
- format the SD card where /dev/mmcblk1 is the device ID
- `sudo mkfs.ext4 /dev/mmcblk1`
-  Create a Directory and give it all the access
- `sudo mkdir -p /mnt`
- `sudo chmod +777 -R /mnt`
- Mount the SD card on this directory
- `sudo mount /dev/mmcblk1 /mnt`
>  give it RW permissions just to be on safer side: `sudo chmod +777 -R /dev/mmcblk1`
- Move all the files
- `sudo rsync -aXS --exclude='/*/.gvfs' /home/  /mnt` 
> make sure **/home/mendel** has all the permissions before attempting rsync  
> `sudo chown -R mendel:mendel /mnt/mendel` where mendel:mendel is the username
- check diff, there should be no output for this, if there is redo this whole process again
- `sudo diff -r /home/ /mnt`
> give write permissions to /etc/fstab
- unmount /mnt
- `sudo umount /mnt`
- mount it as home
- `sudo mount /dev/mmcblk1 /home`
> use `df -h` and check the size of home: it should match SD cards capacity
### this is not persistant , to make it persistant throughout reboots use the following:
- Get the uuid of the sd card:
- `ls -l /dev/disk/by-uuid/`
- we will use the uuid and setup the sd card to be mounted as`/home` with the required boot order
-  open fstab file
- `sudo nano /etc/fstab`
- comment any old `/home` in `/etc/fstab`  and add the following line:
- `UUID=85277668-5307-4cff-b530-f0d08cba4d06   /home   ext4   defaults   0   1`
- save and close the file
- reboot and do `df -h` to check if sd card is persistent



