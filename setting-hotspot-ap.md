If you have a Wi-Fi interface on a computer/laptop connected to the Internet, you may wish to setup a Wi-Fi hotspot.

1. Check if your Wi-Fi interface supports AP interface mode:
  1. Install `iw` package (via pacman for example).
  2. `iw list`:
  ```
  ...
  Supported interface modes:
		 * IBSS
		 * managed
		 * AP      <---- (this means your interface able to be a hotspot)
		 * AP/VLAN
		 * WDS
		 * monitor
		 * mesh point
	...
  ```
2. Install `create_ap` from AUR (using yaourt or any other aur helper). It is possible to do next steps manually, but if you don't need any special configuration using `create_ap` is just fine.
3. See `ip link` to see your Wi-Fi interface, and the interface where your internet comes from (say `eth0`).
4. Run one of:
  1. `create_ap wlan0 eth0 MyAccessPoint` to create a hotspot without a password.
  2. `create_ap wlan0 eth0 MyAccessPoint MyPassPhrase` to create a hotspot with a password.
  3. `create_ap -n wlan0 MyAccessPoint MyPassPhrase` to create a hotspot w/o an Internet sharing. For example to setup a LAN.
