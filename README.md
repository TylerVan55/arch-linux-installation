# Arch Linux Installation Notes

## Starting the Installation

- Created a new virtual machine in VirtualBox and attached the Arch Linux ISO.
- Booted into the Arch installation environment.
- Created two partitions: a 1 GB EFI partition and the remaining space for the Linux system.
- Formatted the EFI partition as FAT32 and the main partition as ext4.
- Mounted the partitions under `/mnt`.

## Installing Arch

- Started installing the base system using `pacstrap`.
- Got an error saying `target not found: textinfo`. I had typed `textinfo` instead of `texinfo`.
- Corrected the package name and ran the installation command again.
- Generated the filesystem table using `genfstab`.
- Entered the installed system using `arch-chroot /mnt`.
- Configured the timezone, clock, locale, hostname, and root password.

## Setting Up GRUB

- Installed GRUB and `efibootmgr`.
- Tried installing GRUB, but got an error saying EFI variables were not supported.
- Went into VirtualBox settings and enabled UEFI.
- Restarted the VM and checked that it booted in UEFI mode.
- Mounted the existing partitions again and entered the installed system using `arch-chroot`.
- Installed GRUB using the removable-media fallback option and generated the GRUB configuration.
- GRUB worked, and I was able to boot from the virtual hard disk.

## Installing the Desktop and Other Packages

- Tried running `pacman -Syu`, but got an error saying there wasn't enough disk space.
- Checked disk usage with `df -h`.
- Realized I was in the live Arch installation environment instead of working in the installed system.
- Mounted the installed partitions and used `arch-chroot /mnt` to enter the installed system.
- Updated the system and installed the packages for NetworkManager, sudo, Nano, Zsh, OpenSSH, LXQt, SDDM, and Xorg.

## Setting Up Users and Permissions

- Edited the sudo configuration with `EDITOR=nano visudo`.
- Enabled sudo access for users in the `wheel` group.
- Created my personal user account and the `codi` account.
- Set passwords for both accounts and configured them to use Zsh.

## Enabling Services

Enabled the following services to start automatically:

```bash
systemctl enable NetworkManager
systemctl enable sshd
systemctl enable sddm
systemctl set-default graphical.target
```

This set up networking, SSH, and the graphical login screen.

## Customizing the Terminal

- Switched to my personal account and opened `~/.zshrc`.
- Added a colored prompt and enabled colored output.
- Created aliases for commands I use frequently.

Some of the aliases I added were:

```bash
alias ll='ls -lah --color=auto'
alias cls='clear'
alias update='sudo pacman -Syu'
alias ports='ss -tuln'
alias myip='ip addr'
```

- Loaded the configuration with `source ~/.zshrc`.
- Tested the `ll` alias.

## Final Result

- Shut down the VM and detached the Arch installation ISO so it would boot from the virtual hard disk.
- Started the VM again.
- The SDDM graphical login screen appeared, and I logged into my personal account.
- Successfully booted into the LXQt desktop.

## Problems I Ran Into

1. **Typo during installation:** Typed `textinfo` instead of `texinfo`. Fixed the package name and continued.
2. **GRUB error:** EFI variables were not supported. Enabled UEFI in VirtualBox and installed GRUB using the removable-media fallback option.
3. **Disk space error:** `pacman -Syu` reported insufficient space because I was in the live installation environment. Mounted the installed system and entered it with `arch-chroot /mnt`.

Overall, I got Arch Linux installed and working in VirtualBox, configured the required accounts and services, and set up the LXQt desktop and customized terminal.
