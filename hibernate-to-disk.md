#Hibernatig on Arch Linux

Hibernation is not an option by default and requires some simple work to establish.

1. Confgiure your boot loader.
  1. With GRUB:
    * `sudo nano /etc/default/grub`
    * Find the string containing `GRUB_CMDLINE_LINUX_DEFAULT='quiet'`. (Instead of quiet there can be anything)
    * Insert `resume=/dev/SWAP` in quotes, where SWAP is your swap partition.
    * My example is `GRUB_CMDLINE_LINUX_DEFAULT='quiet resume=/dev/sda5'`.
    * Save the file (`Ctrl-x, y, Enter`)
  2. Run `sudo grub-mkconfig -o /boot/grub/grub.cfg` to generate grub config.
2. Configure config file of initramfs generator:
  1. Edit the cofig:
    * `sudo nano /etc/mkinitcpio.conf`
    * Find the line that looks like this: `HOOKS="base udev autodetect modconf block filesystems keyboard fsck"`. It is located in the section named HOOKS.
    * After udev insert hook `resume` (Like this: `..base udev resume..`)
    * Save the file (`Ctrl-x, y, Enter`)
  2. Run `mkinitcpio -p linux` to generate initramfs.
3. Use `systemctl hibernate` to hibernate!
