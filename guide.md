# Guide
## Preinstallation

You will need a partition for your Arch Linux installation and, optionally, a swap partition. It is very easy to do before installation, but if you are already loaded into Arch ISO, or doing installation on a new hardware, you can follow these instructions upon loading Arch ISO.

2. `lsblk`. Determine what devices do you have. `/dev/sda` is usually the first SATA drive `/dev/nvme0n1` is usually the first NVMe memory drive (those are SSDs on a motherboard, M.2).
3. `parted device` where `device` is a desired device to have your installation on.
4. Run only if you have an empty (unpartitioned) drive: `mklabel gpt`, where `gpt` is a GPT partition type (UEFI, recommended).
5. `mkpart "EfFI system partition" fat32 1MiB 500MiB
6. `set 1 esp on`
7. `mkpart "Arch" ext4 501MiB 100%`
8. `quit`
9. `lsblk` to check what is there. 1st part is for EFI and 2nd part is for your Linux installation.

## Basic
1. Load arch-linux from your flash drive. Whether it is USB stick or CD-R image.
2. `setfont ter-132b` to set the font bigger if you need it (list is in `/usr/share/kbd/consolefonts`).
3. Make sure you know what your to-be-linux and EFI partitions are. Check Preinstallation section if needed.
  1. You can check `lsblk` or `df` outputs if not sure.
  2. I will call/dev/sdaX — linux partion.
4. `wifi-menu` if you are a wifi user
  1. Choose your network. OK.
  2. OK.
  3. Enter your password for the network. OK.
  4. Wait a little till 'root@archlinuxiso' caption appears.
  5. Don't continue if you cannot connect to a network.
5. `mkfs.fat -F 32 /dev/sdaY` (it's your EFI partition).
  1. Make sure you are formatting the right partitions! **There is NO turning back from this point!**. If yes — enter 'y' when promted.
6. `mkfs.ext4 /dev/sdaX`
7. `mount /dev/sdaX /mnt`
8. `mount /dev/sdaY /mnt/boot`
9. `pacstrap /mnt base base-devel git wget reflector NetworkManager`
  1. Go take a cup of tea. It takes time.
10. `genfstab -p /mnt >> /mnt/etc/fstab`
11. `arch-chroot /mnt`
  1. If promted with sh-4.3 — you are on the right way.
12. `pacman -S {name}` install more packages to your liking.
  1. `dialog wpa_supplicant` -- for wifi access
  4. `fish` -- this a cool and nice looking shell alternative to bash. I recommend it, but it's not POSIX!
13. `reflector -l 30 -f 30 --number 10 --save /etc/pacman.d/mirrorlist --verbose` if you installed reflector
14. `useradd -m -G wheel -s /usr/bin/fish NAME` or `useradd -m -G wheel NAME` if you haven't installed fish
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
22. `bootcltl install`
  1. `systemctl enable --now systemd-boot-update.service`
  2. `cp /usr/share/systemd/bootctl/loader.conf /boot/loader/loader.conf`
  3. `cp /usr/share/systemd/bootctl/arch.conf /boot/loader/entries/arch.conf`
25. `echo compname > /etc/hostname`if you need to change your host name for some reason
26.  Install yay.
  ```bash
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
  ```
27. `exit`
28. `reboot`
29. Take your flash drive out!
30. Login under your user.

## Multiboot

If your Windows system is on the same drive - systemd-boot will handle everything for you. Otherwise there are some additional steps.

1. `pacman -S edk2-shell`
2. `cp /usr/share/edk2-shell/x64/Shell.efi /boot/shellx64.efi` 
3. run `blkid` and take a note (a photo for ex) of PARTUUID for your Windows drive
4. Reboot into EFI Shell (option will be available in systemd-boot menu)
5. Run `map` and take a not of the FS alias for your Windows drive (ex: HD0a66666a2)
6. `exit` and reboot into Arch
7. create a `/boot/windows.nsh` file
  ```
  HD0a66666a2:EFI\Microsoft\Boot\Bootmgfw.efi
  ```
8. create a `/boot/loader/entries/windows.conf`
  ```
title  Windows
efi     /shellx64.efi
options -nointerrupt -noconsolein -noconsoleout windows.nsh
  ```

## X and DE setup
You now have a clean Arch installation. Next steps are for X setup with DE. You can of cource do these steps in the previous section under your `arch-chroot` section.

1. Internet:
  1. `sudo wifi-menu` for Wi-Fi
  2. `sudo systemctl start dhcpcd@.service` for Ethernet
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