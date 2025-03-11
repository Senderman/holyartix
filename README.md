# HolyArtix live ISO

## Custom Artix Linux live ISO build

![2025-03-11_05-00](https://github.com/user-attachments/assets/acdcc181-e806-4a57-b03a-5dcba60b6f88)

## Included packages

- Init System: S6+S6-RC
- BIOS (legacy) loader: syslinux
- UEFI loader: systemdboot (The system itself does not contain systemd)
- Desktop Environment: XFCE4
- Browser: Firefox
- Disk Partitioning Utility: gparted (and also some common utils like fdisk/gdisk etc)
- Disk Recovery Utility: photorec
- BitTorrent Client: qbittorrent
- Network Setup: NetworkManager
- Audio Server: pulseaudio
- Virtual Machine management: qemu+libvirt+virt-manager
- The `switch-distro` script to be able to install both Arch and Artix from this ISO

To see the full list of the installed packages, read [packages.x86_64](packages.x86_64)

## Build

You need a working Artix/Arch host to build the image.

Install [Archiso](https://wiki.archlinux.org/title/Archiso) and run as root:

```bash
mkarchiso -v .
```

Look into the `out/` directory for your newly built ISO image.

## Modify

Before modifying, read the [Archiso](https://wiki.archlinux.org/title/Archiso) article to find answers for the common questions

### Edit package list

Just edit the [packages.x86_64](packages.x86_64) file

### Choose what services will be started on boot

Touch/rm empty files with services' names inside the airootfs/etc/s6/adminsv/default/contents.d directory

### Change contents of the `$HOME`

Modify `airootfs/etc/skel` directory contents

## F.A.Q

**Q:**: What is the login user password?
**A:** `live`. Same as login.
___
**Q:** Cannot use virt-manager 

**A:** To use virt-manager, start `libvirtd` and `virtlogd` with the following command (as root):

`s6-rc start libvirtd && s6-rc start virtlogd`
___
**Q:** How can I install ArtixLinux using this ISO?

**A:** Refer to the [Artix Installation Guide](https://wiki.artixlinux.org/Main/Installation).

After setup, you may need to run the following command (assuming that you've mounted your `/` partition to `/mnt`: 

`rm /mnt/etc/pacman.d/mirrorlist && cp /etc/pacman.d/mirrorlist-artix /mnt/etc/pacman.d/mirrorlist && cp /etc/pacman.d/mirrorlist-arch /mnt/etc/pacman.d && cp /etc/pacman.conf /mnt/etc`

___
**Q:** How can I install ArchLinux using this ISO?

**A:** run as root `switch-distro arch`. This will change contents of `/etc` directory to use ArchLinux's repositories and mirrors.

Then follow the steps of [ArchLinux Installation Guide](https://wiki.archlinux.org/title/Installation_guide), but use `basestrap` instead of `pacstrap`. Everything else should be just the same.

After setup, you may need to run the following command (assuming that you've mounted your `/` partition to `/mnt`: 

`rm /mnt/etc/pacman.d/mirrorlist && cp /etc/pacman.d/mirrorlist-arch /mnt/etc/pacman.d/mirrorlist && cp /etc/pacman-arch.conf /mnt/etc/pacman.conf`
