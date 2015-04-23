# Guide
1. Make sure you know what your to-be-linux and swap partitions are.
  1. We will name them /dev/sdaX — linux partion and /dev/sdaY — swap partition.
2. Load arch-linux from your flash drive. Whether it is USB stick or CD-R image.
3. Upon loading of the live image execute next commands.
4. `wifi-menu`
  1. Choose your network. OK.
  2. OK.
  3. Enter your password for the network. OK.
  4. Wait a little till 'root@archlinuxiso' caption appears.
  5. Don't continue if you cannot connect to a network.
5. `mkswap /dev/sdaY`
6. `swapon /dev/sdaY`
7. `mkfs.ext4 /dev/sdaX`
  1. Make sure you are formatting the right partition! **There is NO turning back from this point!**. If yes — enter 'y' when promted.
8. `mount /dev/sdaX /mnt`
9. `pacstrap /mnt base base-devel`
  1. Go take a cup of tea. It takes time.
10. `genfstab -p /mnt >> /mnt/etc/fstab`
11. `arch-chroot /mnt`
  1. If promted with sh-4.3 — you are on the right way.
12. `pacman -S grub os-prober dialog wpa_supplicant fish`
13. `useradd -m -G wheel -s /usr/bin/fish IMYA`
14. `passwd`
  1. Enter password for root.
15. `passwd IMYA`
  1. Enter password for your user.
16. `nano /etc/sudoers`
  1. Uncomment ...
  2. Save. (Ctrl-X, y, enter)
17. `grub-install --target=i386-pc --recheck --debug /dev/sda`
18. `grub-mkconfig -o /boot/grub/grub.cfg`
19. `exit`
20. `reboot`
21. Take your flash drive out.
22. Login under your user.
23. You now have clean Arch installation. Next steps are for X setup.
24. `sudo wifi-menu`
25. `sudo pacman -S xorg-server xorg-server-utils xorg-apps nvidia sddm xfce4 xarchiver zip unzip firefox`
  1. When asked to choose between nvidia drivers. Choose `nvidia-libgl`.
26. `sudo systemctl enable sddm.service`
27. `sudo reboot`
28. Login under your user.
29. `sudo wifi-menu`
30. You now have clean Arch setup with basic XFCE stuff.
31. Go to http://nale12.deviantart.com/art/Flattastic-11-03-2014-424913255 and download ZIP archive.
32. Unzip it contents into ~/.themes
33. Choose your theme in XFCE -> Appearance and XFCE -> Window Manager
34. To be continued.
