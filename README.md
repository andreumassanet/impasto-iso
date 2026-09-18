# impasto-iso

An install medium for [impasto](https://github.com/andreumassanet/impasto):
Arch Linux, with the desk installed on top, from one USB stick.

It is the official Arch ISO with one thing added. The medium opens
archinstall with the answers filled in — the graphics driver for the card that
is fitted, NetworkManager, PipeWire, Bluetooth — and you answer the rest: the
disk, your user, your keyboard and time zone. The first boot of the new system
then clones impasto and runs `./setup install`, on screen, before the login
screen comes up for the first time.

It installs online: the desk is fetched when it is installed, not carried on
the medium, so the ISO only needs building again when the installer changes.

## Building it

```bash
sudo pacman -S --needed archiso
sudo ./build                  # out/impasto-YYYY.MM.DD-x86_64.iso
sudo ./build --branch dev     # the first boot installs dev instead of main
```

## Writing it to a USB stick

From Linux, with the stick's device (everything on it is erased):

```bash
sudo dd if=out/impasto-*.iso of=/dev/sdX bs=4M status=progress oflag=sync
```

From Windows, Rufus in DD mode, or Ventoy.

## Installing

Boot the stick in UEFI mode. Ethernet connects on its own; for Wi-Fi the
medium opens `iwctl`. It lists the disks first, and says which one Windows is
on, so a machine with two disks keeps Windows on its own: pick the other one
in **Disk configuration**. Then **Authentication** for your user and
**Locales** and **Timezone**, and **Install**.

Reboot when it is done. The first boot takes 10 to 20 minutes, online, and if
something fails it says why and offers to try again. The first time you log
in, a terminal builds the two Hyprland plugins, which can only be built from
inside the session: it asks for your password once.

## Testing it

`./test` runs the medium in a virtual machine (needs `qemu-desktop` and
`edk2-ovmf`, no root):

```bash
./test install                  # a fresh disk, installed with no questions
./test install --local ~/impasto   # the same, installing that checkout
./test boot                     # the first boot, then the login screen
./test shot                     # a picture of the screen
./test stop
```
