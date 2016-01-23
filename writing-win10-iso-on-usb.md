This is probably not the most useful guide, but I wanted to save it at least for myself, cause it is a really convinient way to write any bootable thing on it.

0. Discover your usb's drive letter it is usually b, thus `/deb/sdb`
1. Format a usb drive using either fdisk or gparted or any other tool; give it an NTFS partition.
2. Write down the partition's UUID or label.
3. Mount your Win 10 iso and copy all the files to a usb drive.
  0. mount you usb drive to `<path-to-usb>`.
  1. `mount /dev/sdX mount_point` where `mount_point` is a name of folder to mount to.
  2. `cp -a mount_point path_to_usb`
4. `sudo grub-install --force --target=i386-pc --boot-directory="/<path-to-usb>/boot" /dev/sdX`
5. Create a new file named `grub.cfg` in `/<path-to-usb>/boot/grub/`:
```
default=1
timeout=15
color_normal=light-cyan/dark-gray
menu_color_normal=black/light-cyan
menu_color_highlight=white/black
menuentry "Start Windows Installation" {
 insmod ntfs
 insmod search_fs_uuid
 insmod chain
 search --no-floppy --fs-uuid <drive_UUID> --set root
 chainloader +1
 boot
}
menuentry "Boot from the first hard drive" {
 insmod ntfs
 insmod chain
 insmod part_msdos
 set root=(hd1)
 chainloader +1
 boot
}
```
