# Mounting filesystems on start
1. For ntfs filesystems run `sudo pacman -S ntfs-3g`
2. `sudo blkid`
3. `sudo nano /etc/fstab`
4. Add lines following the next template:

  ```
  # UUID=xxxxxxxxxxxx
  %dev-link%               %mountpoint%     %fs-type%            &options%        0 %fscheck%
  ```
where:
- %dev-link% is a link to your desired filesystem, i.e. /dev/sda1.
- 
