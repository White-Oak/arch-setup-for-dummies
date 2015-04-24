# Guide
## Basic
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
12. `pacman -S grub os-prober dialog wpa_supplicant fish wget`
13. `useradd -m -G wheel -s /usr/bin/fish IMYA`
14. `passwd`
  1. Enter password for root.
15. `passwd IMYA`
  1. Enter password for your user.
16. `nano /etc/sudoers`
  1. Uncomment next line:
  ```
%wheel ALL=(ALL) ALL
  ```
  2. Save. (Ctrl-X, y, enter)
17. `nano /etc/locale.gen`
  1. Uncomment next line (for Russian):
 ```
 ru_RU.UTF-8   UTF-8
 ```
 2. Save. (Ctrl-X, y, enter)
18. `locale-gen`
17. `grub-install --target=i386-pc --recheck --debug /dev/sda`
18. `grub-mkconfig -o /boot/grub/grub.cfg`
19. `exit`
20. `reboot`
21. Take your flash drive out!
22. Login under your user.

## X and Plasma setup
You now have a clean Arch installation. Next steps are for X setup with Plasma 5.

23. `sudo wifi-menu`
24. `sudo pacman -S xorg-server xorg-server-utils xorg-apps nvidia sddm plasma yakuake kdebase-dolphin chromium`
  1. When asked to choose between nvidia drivers. Choose `nvidia-libgl`.
25. `sudo nano /etc/X11/xorg.conf.d/20-keyboard-layout.conf`
  1. Add following lines:
  ```
Section "InputClass"
	Identifier             "keyboard-layout"
	MatchIsKeyboard        "on"
	Option "XkbLayout" "us,ru"
	Option "XkbOptions" "grp:caps_toggle"
EndSection
  ```
  2. Switching will be on Caps lock. To switch on Ctrl-Shift, for example, use `Option "XkbOptions" "grp:ctrl_shift_toggle"`
25. `sudo systemctl enable sddm.service`
26. `sudo reboot`
27. Login under your user.

## Yaourt and custom fonts installation
You now have a clean Arch installation with Plasma 5 with pre-installed Dolphin (file manager), yakuake (drop-down terminal, press F12 in Plasma) and Chromium (web browser).

It is better to perform next steps using Plasma, web browser (i.e. Chromium) and terminal with copy-paste feature supported (i.e. yakuake).

28. Run yakuake or press F12.
29. `sudo wifi-menu`
30. `mkdir aurs-tmp`
31. `cd aurs-tmp`
32. Install package-query from arch AURs:
  1. `wget https://aur.archlinux.org/packages/pa/package-query/package-query.tar.gz`
  2. `tar -xvzf package-query.tar.gz`
  3. `cd package-query`
  4. `makepkg -si`
  5. `cd ..`
33. install yaourt packages:
  1. `wget https://aur.archlinux.org/packages/ya/yaourt/yaourt.tar.gz`
  2. `tar -xvzf package-query.tar.gz`
  3. `cd yaourt`
  4. `makepkg -si`
  5. `cd ..`
26. `sudo pacman -S ttf-bitstream-vera ttf-inconsolata ttf-ubuntu-font-family ttf-dejavu ttf-freefont ttf-linux-libertine ttf-liberation`
27. `yaourt -S ttf-ms-fonts ttf-vista-fonts ttf-monaco ttf-qurancomplex-fonts --noconfirm`
28. `sudo ln -s /etc/fonts/conf.avail/70-no-bitmaps.conf /etc/fonts/conf.d`
29. `sudo reboot`
30. Login under your user.

## Infinality installation
You now have a clean Arch installation with several applications and yaourt and custom fonts installed.

Next steps will guide you through the installion process of an infinality bundle.

30. `sudo nano /etc/pacman.conf`
  1. Add following lines: 
  ```
[infinality-bundle]
Server = http://bohoomil.com/repo/$arch

[infinality-bundle-multilib]
Server = http://bohoomil.com/repo/multilib/$arch

[infinality-bundle-fonts]
Server = http://bohoomil.com/repo/fonts
  ```
  2. Uncomment following lines: 
  ```
[multilib] 
Include = /etc/pacman.d/mirrorlist
  ```
31. `dirmngr < /dev/null`
32. `sudo pacman-key -r 962DDE58`
33. `sudo pacman-key --lsign-key 962DDE58`
34. `sudo pacman -Syy infinality-bundle-multilib`
35. `sudo reboot`
36. Login under your user.
