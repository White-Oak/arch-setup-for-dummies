# Guide
## Preinstallation

You will need a partition for your Arch Linux installation and, optionally, a swap partition. It is very easy to do before installation, but if you are already loaded into Arch ISO, or doing installation on a new hardware, you can follow these instructions upon loading Arch ISO.

1. `lsblk`. Determine what devices do you have. `/dev/sda` is usually the first SATA drive `/dev/nvme0n1` is usually the first NVMe memory drive (those are SSDs on a motherboard, M.2).
2. `parted device` where `device` is a desired device to have your installation on.
3. Run only if you have an empty (unpartitioned) drive: `mklabel gpt`, where `gpt` is a GPT partition type (UEFI, recommended).
4. `mkpart "EFI system partition" fat32 1MiB 2GiB`
5. `set 1 esp on`
6. `mkpart "Arch" ext4 2GiB 100%`
7. `quit`
8. `lsblk` to check what is there. 1st part is for EFI and 2nd part is for your Linux installation.

## Basic

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
7. `mount /dev/sdaX mnt`
8. `mount /dev/sdaY mnt/boot`
9. `pacstrap mnt base base-devel git wget less vim reflector NetworkManager man`
    1. Go take a cup of tea. It takes time.
10. `genfstab -U -p mnt >> mnt/etc/fstab`
11. `arch-chroot mnt`
  a. If prompted with a different shell — you are on the right way.
12. `pacman -S {name}` install more packages to your liking.
  a. `dialog wpa_supplicant` -- for wifi access
  b. `fish` -- this a cool and nice looking shell alternative to bash. I recommend it, but it's not POSIX!
13. `reflector -l 50 -f 5 --protocol https --save /etc/pacman.d/mirrorlist --verbose` if you installed reflector
    1. You can play around with parameters and add `-C 'COUNTRY'` to get results better suited for you.
    2. If you know what you're doing, you can run `reflector` before `pacstrap` and `cp -f {,mnt}/etc/pacman.d/mirrorlist`
14. I recommend editing `echo -e "Color\nParallelDownloads = 5" > /etc/pacman.d/user-friendly.conf` to enable some user-friendly options for pacman.
15. I also recommend adding `echo "PKGEXT='.pkg.tar'" > /etc/makepkg.conf.d/no-compression.conf` to disable compression for AUR packages.
16. `useradd -m -G wheel -s /usr/bin/fish NAME` or `useradd -m -G wheel NAME` if you haven't installed fish
17. `passwd`
  a. Enter password for root.
18. `passwd NAME`
  b. Enter password for your user.
19. `EDITOR=vim visudo`
  a. press `G` to go the end of the file, than `i` and navigate using arrows to the following text. delete `# ` frm the 2nd line. Then `ESC` and `ZZ`
```
## Uncomment to allow members of group wheel to execute any command
# %wheel ALL=(ALL) ALL
```
18. `nano /etc/locale.gen`
  a. Uncomment next line (for Russian): `ru_RU.UTF-8   UTF-8`
  b. Uncomment next line (for English): `en_US.UTF-8   UTF-8`
  c. Save. (Ctrl-X, y, enter)
  d. You can just use `sed` instead of course: `sed -i 's/#\(en_US\.UTF-8\)/\1/' /etc/locale.gen`
19. `echo LANG=en_US.UTF-8 > /etc/locale.conf`
20. `locale-gen`
21. `ln -sf /usr/share/zoneinfo/Europe/Moscow /etc/localtime`, instead of `Europe/Moscow` use your own timezone!
22. `bootcltl install`
  a. `systemctl enable systemd-boot-update.service`
  b. `cp -f /usr/share/systemd/bootctl/loader.conf /boot/loader/loader.conf`
  c. `cp /usr/share/systemd/bootctl/arch.conf /boot/loader/entries/arch.conf` EDIT this file to have `options root=PARTLABEL=Arch add_efi_memmap rw` at the end
25. `echo compname > /etc/hostname`if you need to change your host name for some reason
26.  Install yay.
  ```bash
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
chown NAME:NAME -R .
su NAME
makepkg -si
exit
cd ..
rm -rf yay-bin
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
2. `sudo pacman -S sddm firefox`
  1. For NVIDIA users: run `sudo pacman -S nvidia`.
3. Install DE.
  1. Plasma: `sudo pacman -S plasma-meta yakuake`
4. Choose `noto` for your font options, other choices can be defaults.
3. `sudo systemctl enable sddm.service`
4. `sudo systemctl enable NetworkManager` only if you connect through Wi-Fi
  1. `sudo pacman -S NetworkManager` if it is not installed yet.
5. `sudo reboot`
6. Login under your user. Use networks applet on DE panel to configure network (if you installed NetworkManager).

#### Short DE installation command

`sudo pacman -S sddm firefox nvidia plasma-meta yakuake`
