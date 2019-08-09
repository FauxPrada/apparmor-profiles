# apparmor-profiles

AppArmor profiles for various user applications.

## Notes about usage
These AppArmor profiles have been verified to work on the following hardware:

- CPUs:
    - Intel 4960K
    - Intel 8250U
- GPUs:
    - NVIDIA GeForce GTX 980 Ti
    - Intel 620 UHD Graphics
- Network cards:
	- Intel Wireless-AC 9260

I cannot guarantee that these profiles will work on any other hardware.

All profiles should work with Xorg on NVIDIA hardware and with Wayland (and probably Xorg) on Intel hardware.

⚠️ <--- This symbol means you will need to edit the profile for your specific configuration.

## Tested profiles
### Firefox
From the top of the profile:

```
# Please note: you may have issues with hardware acceleration on NVIDIA hardware. 
# This is because the nvidia_modprobe profile in /etc/apparmor.d/ is configured 
# incorrectly. Change the profile executable name at the top of the 
# nvidia_modprobe profile file (/etc/apparmor.d/nvidia_modprobe) to 
# /usr/bin/nvidia-modprobe.

# Then rename the nvidia_modprobe file to usr.bin.nvidia-modprobe:
# $ mv /etc/apparmor.d/nvidia_modprobe /etc/apparmor.d/usr.bin.nvidia-modprobe
# Don't forget to enforce!
# $ aa-enforce /etc/apparmor.d/*
```

This complex profile has only been tested with the standard `firefox` package as well as the AUR package `firefox-nightly` on Arch Linux, and only with WebRender (on both Intel and Nvidia hardware, on both Wayland and Xorg). This single profile will apply to both Firefox and Firefox Nightly.

You will need to edit this file if your Firefox Nightly files are somewhere other than `/opt/firefox-nightly` (e.g. if you just download the binary from Mozilla's website).

The only directories in the home directory that Firefox is allowed to access are `~/{D,d}ownloads` and `~/.mozilla`. You won't be able to, for example, upload things to the web from your Documents directory. You'll need to copy the file to your downloads directory first.

### KeepassXC ⚠️
This fairly simple profile assumes you keep your database file in `~/{D,d}ocuments/`. If you do not, edit the file to where you store your database. KeepassXC does not need access to your whole home directory. Please keep isolated backups of your database files.

### Lollypop
This fairly simple profile assumes you keep your music in `~/{M,m}usic`. If you do not, edit the file to where you store your database. Lollypop does not need access to your whole home directory.

I have not tested the profile for any web features, so they probably will not work.

### mpv
From the top of the profile:

```
# Please note: you may have issues with hardware decoding on NVIDIA hardware. 
# This is because the nvidia_modprobe profile in /etc/apparmor.d/ is configured 
# incorrectly. Change the profile executable name at the top of the 
# nvidia_modprobe profile file (/etc/apparmor.d/nvidia_modprobe) to 
# /usr/bin/nvidia-modprobe.

# Then rename the nvidia_modprobe file to usr.bin.nvidia-modprobe:
# $ mv /etc/apparmor.d/nvidia_modprobe /etc/apparmor.d/usr.bin.nvidia-modprobe
# Don't forget to enforce!
# $ aa-enforce /etc/apparmor.d/*
```

This fairly complex profile also allows mpv to utilize youtube-dl. I have also verified that this AppArmor profile works when mpv is invoked by other programs (like [streamlink](https://streamlink.github.io/)).

Use the command line flag `--gpu-context=wayland` for Wayland support.

Use the command line flag `--hwdec=auto` for nvdec (NVIDIA) and VA-API (Intel) hardware decoding.

## 🛑 New profiles
These profiles are new, and somewhat untested. Use at your own risk.

### gpg-agent
A relatively simple profile for the standard `gpg-agent`. This profile has only been tested with the gtk2 pinentry program. Please keep isolated backups of your GPG keys.

### iwd
Read more about `iwd`: https://wiki.archlinux.org/index.php/Iwd

An extremely simple profile for the new NetworkManager wireless backend `iwd` but very untested. Wifi connections, Wireguard connections, switching Wifi networks, etc. all work on my machine with an Intel 9260AC. Use at your own networking peril.

### mako
An extremely simple profile for the Wayland-native notification daemon, `mako`. You may need to edit the profile to allow `mako` to access your configuration file, if it's a symlink to somewhere other than inside `~/.config/mako/`.

### NetworkManager
A relatively simple profile for the standard `NetworkManager` but very untested. Wifi connections, Wireguard connections, switching Wifi networks, etc. all work on my machine with an Intel 9260AC. Use at your own networking peril.

### redshift
This extremely simple profile has been tested to work correctly on Wayland (sway) with the `redshift-wlr-gamma-control` AUR package. 

### ssh-agent
An extremely simple profile for the standard `ssh-agent` (no pinentry).

### swaybg ⚠️
From the top of the profile: 
```
# Please note: you may need to edit this file to specify the location of your
# wallpaper!
```

Otherwise an extremely simple profile for the default sway background setter program `swaybg`.

### syncthing ⚠️
Read more about syncthing: https://syncthing.net/

A fairly simple profile for the wonderful decentralized file synchronization service Syncthing. This profile has not been tested extensively. Please keep isolated backups of your data, regardless of whether or not you use this profile.

From the top of the profile:
```
# Please note: you will need to edit this file to allow syncthing to access your
# personal synced directories.
```

### waybar ⚠️
From the top of the profile:
```
# Please note: this is an AppArmor profile for my personal setup and is only 
# meant to be used with the modules I use, so may prevent some modules from 
# loading on your setup. You will need to write your own personal rules if you 
# use different modules.

# Modules tested to work:
#   sway/workspaces, sway/mode, sway/window, network, pulseaudio, cpu, clock
```

Otherwise a fairly simple profile.

## Contributing
Pull requests and issues are welcome. I cannot test for hardware I do not have access to (AMD), so those PRs would be most critical.
