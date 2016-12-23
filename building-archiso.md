# Building your own Arch installation ISO
This is really well covered in https://wiki.archlinux.org/index.php/archiso, but I will illustrate it in a step-by-step way on my exact example.

1. `sudo pacman -S archiso`
2. `sudo -i`
3. `mkdir archiso`
4. `cp -r /usr/share/archiso/configs/releng/* ~/archiso`
5. `cd archiso`
6. `nano build.sh`. This is a building script for an ISO, edit it to shape your distibution, here's what I did:
  1. Remove all `i686` from the file
  2. Replace `40M` in `truncate -s 40M` with `50M`. No idea why, but 40 MBs is just not enough.
7. I needed a `broadcom-wl` package, so my Wi-Fi card would work in the ISO:
  1. Launch another terminal, since it is not possible to run `makepkg` in root envinronment.
  2. `wget https://aur.archlinux.org/cgit/aur.git/snapshot/broadcom-wl.tar.gz`
  3. `tar -xvf broadcom-wl.tar.gz`
  4. `cd broadcom-wl`
  5. `makepkg -s`. `-s` means install dependencies. It is possible to do it here, because `linux-headers` depndency of the package is make-dependent, so I don't really need it in my ISO. Always check the needed depndencies, when adding custom packages.
  6. Leave the resulting package in the directory or copy it anywhere, but remember the path!
  7. `mkdir custompkg/x86_64`
  8. Copy the built package into that folder
  9. `repo-add custompkgs/x86_64/custompkgs.db.tar.gz custompkgs/x86_64/broadcom-wl*`
  10. Return to your root shell
  11. `nano pacman.conf` and add (replace `/root/` with a path to to your `custompkgs`)
  ```
   [custompkgs]
   SigLevel = Optional TrustAll
   Server = file:///root/custompkgs/$arch
  ```
  12. Next two lines are only needed, because I need to disable two kernel modules. But if you have some iso-building-time execution required, place it in `airootfs/root/customize_airootfs.sh`:
  ```
  echo "blacklist b43" >> /etc/modprobe.d/blacklist.conf
  echo "blacklist ssb" >> /etc/modprobe.d/blacklist.conf
  ```
8. `nano packages.x86_64`, and add whatever packages you need. I added `fish` and `python2`.
9. You are ready to go! `./build.sh -v`
  
