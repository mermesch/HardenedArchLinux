# Linux installation and hardening guide

*Arch Linux, Full Disk Encryption, Secure Boot and extensive hardening

Most Linux security guides are basic, outdated, or incomplete. Very few actually show you how to build a secure system from scratch. This guide does.

## How this setup works

This guide uses a very strict setup for maximum security and maximum privacy. The main computer (the host) is completely locked down and only used for system updates. All your actual daily work is done inside a Virtual Machine (VM).

If you do not want to use VMs, the guide includes a few simple tweaks so you can safely use the host computer as your workspace.

Hardening of end user applications, like chat messengers and browsers, is out of scope for this guide. This guide only covers host hardening for VM use.

## Who is this for?

You do not need to be an activist, privacy advocate, journalist or work for a high-risk company to use this guide. It is for anyone. There is nothing wrong with having stronger security than you think you need. You can never predict the tools, skills, or patience of an attacker you might face in the future.

## You are in control

This guide shows you a strict path, but you are not forced to do everything. You can read through it and easily decide which security measures you want to apply and which ones you want to skip.

## Status of this guide

**Disclaimer:** I currently use Pop!_OS myself. I wrote this installation and hardening guide for Arch Linux a while ago and had no time to finish it for a long time.

The **installation part** was tested a year ago, but not recently. You might face a few errors. The **hardening part** is up to date.

Some hardening measures I use myself and recommend in this guide, but I did not include instructions for them. For example, I always use sdwdate instead of chrony. But maintaining installation instructions for sdwdate on Arch Linux is a lot of work. I can make it work for Arch, but it can be broken again in a few months. If you want maximum privacy, you should take care of time synchronization yourself.

## Prefer a ready-made hardened distro?

If you are looking for a hardened Linux distro but don't have the time or skills to create it, I recommend KickSecure or Secureblue:

- If you are looking for **security and privacy**, you should pick KickSecure.
- If you want **maximum security**, you should pick Secureblue.
- Secureblue has immutable parts. If you want a secure distro and still want to be able to tweak everything in the OS, I would also pick KickSecure.

You can also use KickSecure or Secureblue for one of your VMs after installing the host with this guide. Without the work of KickSecure and Secureblue, I would not have been able to write this guide. Consider donating to them for all their work.

## Read everything, do not copy-paste

There are some hardening parts that I did not explain in depth, or explain what they exactly are. If I think you might want to skip some of the hardening, I did explain why you might want to disable it. **To not miss these notes, you have to read everything and not copy-paste without reading.**

## Guide contents

1_COMPUTER_LAPTOP_SELECTION - Hardware threats, laptop choice, its components and your physical habits affect security and anonymity.
2_OS_SELECTION - Operating system selection.
3_INSTALLATION - Arch Linux installation with very strong full disk encryption using argon2id and a high iter-time. GRUB is used to make boot partition encryption possible.
4_DESKTOP_ENVIRONMENT - Desktop environment selection. Why Wayland is required instead of X11. GNOME, KDE and LXQt.
5_NETWORK - Network configuration using a strictly hardened NetworkManager systemd service. MAC randomization. IPv6 disabled. Static IP with no DHCP client.
6a_SECUREBOOT - Secure Boot.
6b_SECUREBOOT_SHIM - Secure Boot with SHIM.
7_FIRMWARE_UPDATE - Firmware update.
8_PRE_INSTALLATION_INFORMATION - AppArmor, systemd sandboxing, post-quantum crypto, minimalism.
9_VPN - VPN selection and configuration. Prevent DNS leaks.
10_SOME_APPS_AND_HARDENING - Base packages, AppArmor, user account, and masking unnecessary services.
11_CURL_APPARMOR - Pacman downloads under the curl AppArmor profile.
12_HARDENING_KARGS - Kernel hardening via GRUB kernel arguments.
13_SYSCTL - sysctl hardening for kernel, userspace, and networking.
14_MODULE_HARDENING - Kernel module blacklisting.
15_CHRONY - NTS time synchronization with a hardened chronyd service.
16_TOR - Tor with AppArmor.
17_PACMAN_TOR - Pacman downloads through Tor.
18_REFLECTOR - Mirrorlist generation via torsocks, HTTPS mirrors only.
19_PASSWORD - Password hardening: pwquality, yescrypt, faillock.
20_FSTAB - Hardened mount options and tmpfs for logs and caches.
21a_GNOME_HARDENING - GNOME hardening.
21b_KDE_HARDENING - KDE Plasma hardening
22_DISABLE_COREDUMPS - Disable core dumps.
23_SYSTEM_MAP - System.map shredded at every boot.
24_USBGUARD - USBGuard: only whitelisted USB devices work, unknown or tampered devices are blocked.
25_PERMISSIONS - Permission hardening, SETUID whitelist and file capabilities set where needed, reapplied after every update by a pacman hook.
26_VIRT_MANAGER - VMs with virt-manager/QEMU in the unprivileged user session. Clock randomized at boot and kvm-clock disabled to defeat time-based fingerprinting, passt network stack confined with AppArmor.
27_HARDENED_MALLOC - Hardened memory allocator preloaded system-wide, making heap exploitation much harder.
28_BASH_HISTORY - Bash history disabled for all users and shells, leaving no command traces on disk when /var/log is mounted as tmpfs.
29_POST_INSTALLATION_HARDENING - Post-installation checks and maintenance.

## Warning

No one should feel immortal with a hardening system. Hardened Linux is still flawed. Tor is mostly compromised, VPNs can be malicious and can be bypassed by taps at the hosting provider layer of the server. Any security or privacy tool you are going to use has its own security or privacy flaws. Without a deep understanding of the underlying technology, you may use something insecure, use it in an insecure way, or have a great chance of leaking your identity. Never trust AI when it's about security or privacy. Most information is false or incorrect.
