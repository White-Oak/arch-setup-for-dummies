# Guide
1. wifi-menu

 1.1. Choose your network. OK.
 1.2. OK.
 1.3. Enter your password for the network. OK.
 1.4. Wait a little till 'root@archlinuxiso' caption appears.
 1.5. Don't continue if you cannot connect to a network.
2. Make sure you know what your to-be-linux and swap partitions are.

 2.1. We will name them /dev/sdaX — linux partion and /dev/sdaY — swap partition.
3. Load arch-linux from your flash drive. Whether it is USB stick or CD-R image.
4. Upon loading of the live image execute next commands.
5. mkswap /dev/sdaY
6. swapon /dev/sdaY
7. mkfs.ext4 /dev/sdaX 

 7.1. Make sure you are formatting the right partition! _There is NO turning back from this point!_. If yes — enter 'y' when promted.
8. mount /dev/sdaX /mnt
9. pacstrap /mnt base base-devel

 9.1. Go take a cup of tea. It takes time.
10. genfstab /mnt >> /etc/fstab
11. arch-chroot /mnt

 11.1. If promted with sh-4.3 — you are on the right way.
12. pacman -S grub os-prober dialog wpa_supplicant nvidia xorg-server xorg-server-utils xorg-apps sddm fish
13. useradd -m -G wheel -s /usr/bin/fish IMYA
14. passwd

 14.1. Enter password for root.
15. passwd IMYA

 15.1. Enter password for your user.
16. nano /etc/sudoers
 16.1. Uncomment ...
17. grub-install --target=i386-pc --recheck --debug /dev/sda
18. grub-mkconfig -o /boot/grub/grub.cfg
19. exit
20. reboot
21. to be continued.
