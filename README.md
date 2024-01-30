# Disk Expansion Documentation
These are the steps and commands to follow to resize the disk to 4TB.

![MicrosoftTeams-image (2)](https://github.com/jcontreras07/DiskExpansion/assets/143835358/71cde8ad-5151-4ec0-ae25-b3982e76287a)

The disk `nvme0n1` has a total space of 3.9T (2T were acquired recently) however, the partition that the server uses is `nvme0n1p1`, it is therefore needed to extend the size of the existing partition to utilize the extra space on the NVMe device `nvme0n1`.

### 1. Backup Your Data:
Before making any changes to partitions, it's crucial to ensure that you have a backup of your important data.


### 2. Restart The Server:
The command to restart a Linux server is `reboot`. When you run this command, it initiates a system reboot, causing the operating system to shut down gracefully, close all running processes, and then restart the system. Here's how you can use the `reboot` command:

`sudo reboot`

`sudo`: The sudo command is used to execute the reboot command with administrative privileges. You might need superuser (root) privileges to perform a system reboot.

`reboot`: This is the command itself. When executed with sudo, it triggers the process of restarting the system.

Before running the reboot command, make sure to save any unsaved work and ensure that you won't cause any disruptions, especially if you're working on a remote server.
After performing a reboot, it's a good practice to update your system to ensure that it has the latest security patches, bug fixes, and updates. The standard commands for updating and upgrading packages are:

`sudo apt update`

This command refreshes the package lists from the repositories, providing information about the latest available versions of packages.

`sudo apt upgrade`

This command upgrades the installed packages to their latest versions. It's a good idea to run this after apt update.

### 3. Check for Available Space:

Confirm that there is unallocated space on the NVMe device. You can use a tool like `parted` or `gparted` for a graphical interface:

`sudo parted /dev/nvme0n1 print`

Look for unallocated space on the NVMe device.

### 4. Resize the Partition:

You can use a tool like `gparted` to resize the existing partition (`nvme0n1p1`). Install `gparted` if you don't have it:

`sudo apt-get install gparted`

Run `gparted`:

`sudo gparted`

* Select the NVMe device (nvme0n1) in the top-right corner.
* Locate the partition (nvme0n1p1) and resize it to include the unallocated space.
* Apply the changes.

### 5. Resize the Filesystem:

After resizing the partition, you'll need to resize the filesystem to make use of the additional space. If the partition is currently mounted, you may need to unmount it first.

`sudo umount /dev/nvme0n1p1`

Resize the filesystem:

`sudo resize2fs /dev/nvme0n1p1`

If you are using an ext4 filesystem. If you are using a different filesystem type, use the appropriate tool (xfs_growfs, resize2fs, etc.).

### 6. Remount the Partition:

After resizing the filesystem, remount the partition:

`sudo mount /dev/nvme0n1p1 /path/to/mount/point`

### 7. Verify the Changes:

Verify that the partition has been resized and is now using the additional space:

`df -h`

Confirm that the partition now reflects the increased size.

Remember to adapt the commands to your specific setup, and ensure you are working on the correct partitions to avoid data loss. If you are uncomfortable with these operations, consider seeking assistance from someone experienced with disk partitioning.
