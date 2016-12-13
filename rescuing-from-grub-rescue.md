# Rescuing when screwed up grub.cfg
Sometimes I fuck up grub.cfg either by modifying partitions or editing it by hand and the my system welcomes me with a cold `grub rescue>`. At these moments I sigh deeply and search for my USB drive with a linux on it, to boot and run `grub-mkconfig -o /boot/grub/grub.cfg`. Once I hadn't that drive by me and I panicked.  
But in relity it wasn't that hard to fix stuff from the rescue shell. Follow me:

1. `ls`
2. You will see a list of your partitions; using `ls (hd0,msdos1)` try them one by one to ensure you can identify your root partition.
3. `set prefix=(your_drive,your_part)/boot/grub`
4. `set root=(your_drive,your_part)`
5. `insmod normal`
6. `normal`. Here you will have tab-autocmpletion and history enabled for you.
7. `insmod linux`
8. `linux /boot/vmlinuz root=/dev/sda1 rw quiet`. Your image name can differ.
9. `initrd /boot/initramfs-linux.img`. Again, your image name can differ.
10. `boot`
