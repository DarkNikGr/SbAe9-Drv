# Friendly AE-9 names

Optional files for a consistent name on the card, the output and every
input of the 3xHelix driver.

| File | Installed as | What it names |
|---|---|---|
| `60-ae9-rename.conf` | `~/.config/wireplumber/wireplumber.conf.d/60-ae9-rename.conf` | **Sound Blaster AE-9 Output**, **Sound Blaster AE-9 Microphone** |
| `90-sb-ae9-name.rules` | `/etc/udev/rules.d/90-sb-ae9-name.rules` | the card itself: **Sound Blaster AE-9** |
| `ae9-optical-in.conf` | `~/.config/pipewire/pipewire.conf.d/ae9-optical-in.conf` | **Sound Blaster AE-9 Optical In** |
| `ae9-what-u-hear.conf` | `~/.config/pipewire/pipewire.conf.d/ae9-what-u-hear.conf` | **Sound Blaster AE-9 What U Hear** |

The stock 3xHelix `61-ae9-names.conf` only names the microphone; this
set covers the output and all inputs.  `60-ae9-rename.conf` includes the
same microphone rule, so you can remove the old file.

## Install

```sh
mkdir -p ~/.config/wireplumber/wireplumber.conf.d
mkdir -p ~/.config/pipewire/pipewire.conf.d
cp config/60-ae9-rename.conf ~/.config/wireplumber/wireplumber.conf.d/
cp config/ae9-optical-in.conf config/ae9-what-u-hear.conf \
   ~/.config/pipewire/pipewire.conf.d/
sudo cp config/90-sb-ae9-name.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules
sudo udevadm trigger --subsystem-match=sound
systemctl --user restart wireplumber pipewire
```

If the old file is still there and you want only one source of names:

```sh
rm -f ~/.config/wireplumber/wireplumber.conf.d/61-ae9-names.conf
```

## Revert

```sh
rm -f ~/.config/wireplumber/wireplumber.conf.d/60-ae9-rename.conf
rm -f ~/.config/pipewire/pipewire.conf.d/ae9-optical-in.conf \
      ~/.config/pipewire/pipewire.conf.d/ae9-what-u-hear.conf
sudo rm -f /etc/udev/rules.d/90-sb-ae9-name.rules
sudo udevadm control --reload-rules
sudo udevadm trigger --subsystem-match=sound
systemctl --user restart wireplumber pipewire
```

The node names (`alsa_output.pci-0000_29_00.0...`,
`alsa_input.ae9_optical`, `alsa_input.ae9_what_u_hear`) do not change;
only the desktop descriptions and the card model do.  Applications and
SbAe9-SBC keep finding the devices by node name.
