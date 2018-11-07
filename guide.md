# Guide
## Preinstallation
1. You will need a partition for your Arch Linux installation and, preferably, a swap partition. It is very easy to do before installation, but if you are already loaded into Arch ISO, or doing installation on a new hardware, you can follow these instructions.
2. `lsblk`. Determine what devices do you have. `/dev/sda` is usually the first SATA drive `/dev/nvme0n1` is usually the first NVMe memory drive (those are SSDs on a motherboard).
3. `parted device` where `device` is a desired devic to have your installation on.
4. Run only if you have an empty (unpartitioned) drive: `mklabel msdos`, where `msdos` is a MBR partition type.
5. `mkpart primary ext4 0% 100%`
6. `quit`
7. `lsblk` to check what is there.

## Basic
1. Make sure you know what your to-be-linux and swap partitions are.
  1. We will name them /dev/sdaX — linux partion and /dev/sdaY — swap partition.
2. Load arch-linux from your flash drive. Whether it is USB stick or CD-R image.
3. Upon loading of the live image execute next commands.
4. `wifi-menu` if you are wifi user
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
12. `pacman -S grub os-prober` for MBR loader and add more:
  1. `dialog wpa_supplicant` -- for wifi access
  2. `wget` -- we will need this to get yaourt
  3. `reflector` -- this only updates mirror list, pretty optional
  4. `fish` -- this a cool and nice looking shell alternative to bash. I recommend it, but it's not POSIX!
13. `reflector --latest 50 --number 10 --sort rate --save /etc/pacman.d/mirrorlist --verbose` if you installed reflector
14. `useradd -m -G wheel -s /usr/bin/fish IMYA` or `useradd -m -G wheel IMYA` if you haven't installed fish
15. `passwd`
  1. Enter password for root.
16. `passwd IMYA`
  1. Enter password for your user.
17. `nano /etc/sudoers`
  1. Search for:
  ```
## Uncomment to allow members of group wheel to execute any command
# %wheel ALL=(ALL) ALL
  ```
  2. Uncomment `%wheel ALL=(ALL) ALL`
  3. Save. (Ctrl-X, y, enter)
18. `nano /etc/locale.gen`
  1. Uncomment next line (for Russian): `ru_RU.UTF-8   UTF-8`
  2. Uncomment next line (for English): `en_US.UTF-8   UTF-8`
  3. Save. (Ctrl-X, y, enter)
  4. You can just use `sed` instead of course: `sed -i 's/#\(en_US\.UTF-8\)/\1/' /etc/locale.gen`
19. `echo LANG=en_US.UTF-8 > /etc/locale.conf`
20. `locale-gen`
21. `ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime`, instead of `Europe/Moscow` use your own timezone!
22. `grub-install --target=i386-pc --recheck --debug /dev/sda`
23. `grub-mkconfig -o /boot/grub/grub.cfg`
25. `echo compname > /etc/hostname`if you need to change your host name for some reason
26. `exit`
27. `reboot`
28. Take your flash drive out!
29. Login under your user.

## X and DE setup
You now have a clean Arch installation. Next steps are for X setup with DE.

1. Internet:
  1. `sudo wifi-menu` for Wi-Fi
  2. `sudo systemctl enable --now dhcpcd@.service` for Ethernet
2. `sudo pacman -S xorg-server xorg-server-utils xorg-apps sddm chromium`
  1. For NVIDIA users: When asked to choose between nvidia drivers. Choose `nvidia-libgl`. Run `sudo pacman -S nvidia`.
  2. For Intel GPU users: run `sudo pacman -S xf86-video-intel mesa-libgl libva` 
  3. For Mixed users: run `sudo pacman -S bumblebee`
3. Install DE.
  1. Plasma: `sudo pacman -S plasma yakuake dolphin gtk-theme-orion`
  2. XFCE: `sudo pacman -S xfce4`
3. `sudo systemctl enable sddm.service`
4. `sudo systemctl enable NetworkManager` only if you connect through Wi-Fi
  1. `sudo pacman -S NetworkManager` if it is not installed yet.
5. `sudo reboot`
6. Login under your user. Use networks applet on DE panel to configure network (if you installed NetworkManager).

## Yaourt installation

1. `mkdir aurs-tmp`
2. `cd aurs-tmp`
3. Install package-query from arch AURs:
  1. `wget https://aur.archlinux.org/cgit/aur.git/snapshot/package-query.tar.gz`
  2. `tar -xvzf package-query.tar.gz`
  3. `cd package-query`
  4. `makepkg -si`
  5. `cd ..`
4. install yaourt packages:
  1. `wget https://aur.archlinux.org/cgit/aur.git/snapshot/yaourt.tar.gz`
  2. `tar -xvzf yaourt.tar.gz`
  3. `cd yaourt`
  4. `makepkg -si`
  5. `cd ..`
5. `cd ..`
6. `rm -rf aurs-temp`

## Custom fonts installation
You now have a clean Arch installation with Plasma 5 with pre-installed Dolphin (file manager), yakuake (drop-down terminal, press F12 in Plasma) and Chromium (web browser).

It is easier to perform next steps using DE, web browser (i.e. Chromium) and terminal with copy-paste feature supported (i.e. yakuake or konsole).

1. Run yakuake or press F12.
2. Run following to change your shortcut to change languages (if you need one):
  1. `sudo nano /etc/X11/xorg.conf.d/20-keyboard-layout.conf`
  2. Add following lines. Switching will be on Caps lock. To switch on Ctrl-Shift, for example, use `Option "XkbOptions" "grp:ctrl_shift_toggle"`:
  ```
Section "InputClass"
	Identifier             "keyboard-layout"
	MatchIsKeyboard        "on"
	Option "XkbLayout" "us,ru"
	Option "XkbOptions" "grp:caps_toggle"
EndSection
  ```
3. `sudo pacman -S ttf-bitstream-vera ttf-inconsolata ttf-ubuntu-font-family ttf-dejavu ttf-freefont ttf-linux-libertine ttf-liberation`
4. For Microsoft fonts:
  1. `yaourt -S ttf-ms-fonts ttf-vista-fonts ttf-monaco ttf-qurancomplex-fonts --noconfirm`
5. `sudo ln -s /etc/fonts/conf.avail/70-no-bitmaps.conf /etc/fonts/conf.d`
6. `sudo reboot`
7. Login under your user.
