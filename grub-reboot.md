GRUB contains the ability to reboot into a specified OS once. This is useful for people who use dual boot.

## Configuration
1. Set `GRUB_DEFAULT` in `/etc/default/grub` to `GRUB_DEFAULT=saved`.
2. `sudo grub-mkconfig -o /boot/grub/grub.cfg` to regenerate GRUB config,
3. `sudo grub-set-default 0` Instead of 0 put your convenient Arch system.

## Usage
1. `sudo grub-reboot 3` Instead of 3 put the number of menuentry of your boot-once-into system. 
2. `systemctl reboot`
