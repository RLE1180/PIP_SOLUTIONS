**Task 1: Creating Logical Volumes**

1. **List Available Storgae Devices** : 

You can use either `lsblk` or `fdisk -l` to list available storgage devices. Example : 

**lsblk**

![[Screenshot from 2023-11-03 15-16-36.png]]

**fdisk -l**

![[Screenshot from 2023-11-03 15-18-43.png]]
2. **Create a new partition** : 

Use the `fdisk` command to create a new partition on the device. For example,

**sudo fdisk /dev/sda**  # press 'm' for help, press 'n' to create a new partition, follow the prompts and 'w' to write changes.

![[Screenshot from 2023-11-07 09-58-53.png]]

![[Screenshot from 2023-11-07 09-59-21.png]]

3. **Initialize the New Partition as a Physical Volume (PV) for LVM:**

Use the `pvcreate` command to initialize the new partition as a PV:

**sudo pvcreate /dev/sda3**  # Replace /dev/sda3 with the actual partition name.

4. **Create a Volume Group (VG):**

Create a VG using the `vgcreate` command and add the PV to it:

**sudo vgcreate my_volume_group /dev/sda3** # Replace 'my_volume_group' with your desired VG name.

5. **Create Logical Volumes (LVs):**

Create two LVs within the VG using the `lvcreate` command:

- One LV for the root filesystem:

**sudo lvcreate -L 10G -n root_lv my_volume_group** # Replace '10G' with the desired size and 'root_lv' with the LV name.

- Another LV for user data:

**sudo lvcreate -L 20G -n user_data_lv my_volume_group** # Replace '20G' with the desired size and 'user_data_lv' with the LV name.

**Task 2: Filesystem Creation and Mounting**

1. **Format the LVs with Filesystems** : 

Use the `mkfs` command to format the LVs with the desired filesystem. For example,

**sudo mkfs.ext4 /dev/my_volume_group/root_lv**

2. **Create Mount Directories** : 

Create directories where you want to mount the LVs. For example:

**sudo mkdir /mnt/root_lv** 
**sudo mkdir /mnt/user_data_lv**

![[Screenshot from 2023-11-07 11-25-02.png]]

3. **Mount the LVs** : 

Use the `mount` command to mount the LVs to the corresponding directories. For example:

**sudo mount /dev/my_volume_group/root_lv /mnt/root_lv** 
**sudo mount /dev/my_volume_group/user_data_lv /mnt/user_data_lv**

 # Replace `/dev/my_volume_group/root_lv` and `/dev/my_volume_group/user_data_lv` with the paths to your LVs

4. **Verify Mounting** : 

You can verify that the LVs are mounted correctly using the `df` and `mount` commands:

- Use `df` to display information about disk space usage:
    
    **df -h**
    
    This will show you the mounted LVs and their available space.
![[Screenshot from 2023-11-07 11-31-58 1.png]]

- Use `mount` to display information about mounted filesystems:
    
    **mount**
    This will show you the list of mounted filesystems, including the LVs and their mount points.

**Task 3: Resizing Logical Volumes**

1. **Extend the Size of the LV using `lvextend`**:

**sudo lvextend -L +5G /dev/my_volume_group/user_data_lv**
 
Replace `/dev/my_volume_group/user_data_lv` with the LV you want to extend and adjust the size (in this example, we're extending it by 5GB).

2. **Resize the Filesystem using `resize2fs`**: 

**sudo resize2fs /dev/my_volume_group/user_data_lv**

3. **Confirm the LV and Filesystem Resizing**:

To confirm that the LV and filesystem have been successfully resized, you can use the following commands:

- To check the LV size:

**sudo lvdisplay /dev/my_volume_group/user_data_lv**

This will display information about the LV, including its size.

- To check the filesystem size:

**df -h /mnt/user_data_lv**

This command will show you the new size of the filesystem.

**Task 4: Snapshot Creation**

1. **Create a Snapshot of the Root LV** :

Use the `lvcreate` command with the `--snapshot` option to create a snapshot of the root LV. For example:

bashCopy code

**sudo lvcreate --snapshot -L 2G -n root_snapshot /dev/my_volume_group/root_lv**

This command creates a snapshot LV named `root_snapshot` that is 2GB in size. Adjust the size and names according to your requirements.

2. **Mount and Explore the Snapshot** :

You can mount the snapshot LV to a temporary directory and explore its contents:

**sudo mkdir /mnt/root_snapshot**  
**sudo mount /dev/my_volume_group/root_snapshot /mnt/root_snapshot**

3. **Snapshot and Its Use Cases** : 

A snapshot is a point-in-time, read-only copy of a LV that allows you to:

- **Backup Data:** You can use snapshots to create consistent backups without interrupting the original LV's operation.
- **System Recovery:** Snapshots can be used for system recovery or rollback to a known state.
- **Testing:** Snapshots are useful for testing software updates or changes without risking the original data.
- **Data Analysis:** They can be used for data analysis or auditing purposes without altering the primary data.
-  **Reducing Downtime:** Snapshots minimize downtime when you need to resize, move, or make changes to the LV, as you can perform these tasks on the snapshot.

**Task 5: Cleanup**

1. **Unmount LVs and Snapshots** : 

Unmount the LVs and snapshots using the `umount` command. For example:

**sudo umount /mnt/root_lv 
sudo umount /mnt/user_data_lv 
sudo umount /mnt/root_snapshot**

2. **Remove LVs and VGs:**

Use the `lvremove` command to remove the LVs and the `vgremove` command to remove the VG. For example:

**sudo lvremove /dev/my_volume_group/root_lv  
sudo lvremove /dev/my_volume_group/user_data_lv 
sudo lvremove /dev/my_volume_group/root_snapshot **

**sudo vgremove my_volume_group**

Replace `my_volume_group` with the actual name of your Volume Group.

3. **Optionally Remove PV and Undo Partitioning** :

If you wish to remove the PV and undo partitioning, you can do so using the following steps:

- First, remove the PV:
    
    **sudo pvremove /dev/sda3**
    
    Replace `/dev/sda31` with the actual PV you want to remove.
    
- If you want to remove the partition itself, you can use a partitioning tool like `fdisk` or `parted`:
    
    **sudo fdisk /dev/sda**
 # Use the 'd' option to delete the partition.
 # Then, use 'w' to write the changes.
 