# Mounting file systems on start
1. For ntfs file systems run `sudo pacman -S ntfs-3g`
2. `sudo blkid`
3. `sudo nano /etc/fstab`
4. Add lines following the next template:

  ```
  # UUID=%UUID%
  %dev-link%               %mountpoint%     %fs-type%            %options%        0 %fscheck%
  ```
  
where:
* %UUID% is an UUID from blkid output.
* %dev-link% is a link to your desired file system, i.e. /dev/sda1.
* %mountpoint% is a desired location to mount a file system, i.e. /home/backup
* %fs-type% is a type of file system, i.e. ntfs-3g or ext4
* %options% is a field for options
  * Recommended options for ntfs-3g filesystems are `defaults`
  * Recommended options for ext4 file systems are `rw,relatime,data=ordered` 
* %fscheck% is a number telling fschk whether this drive should be checked at a start of a system.
  * 0 -- don't check
  * 1 -- root file system
  * 2 -- check
  
# Example
As an example I provide my own fstab file:

```
# UUID=488864ca-9a93-4ff5-99a7-9d01435efd69
/dev/sda6               /               ext4            rw,relatime,data=ordered        0 1

# UUID=009d54dc-9471-401e-8b85-2bd29d986a50
/dev/sda5               none            swap            defaults                        0 0

# UUID=F8CCE799CCE75104
/dev/sda3               /data           ntfs-3g         defaults                        0 0

# UUID=972e7a3b-aab8-49be-b747-858ff6356ff1
/dev/sda2               /unrelated      ext4            rw,relatime,data=ordered        0 2
```
