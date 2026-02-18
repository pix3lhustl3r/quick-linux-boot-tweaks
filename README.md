# Make Your Linux Boot Super Fast
(for Linux Mint, LMDE, Debian – especially old/slow laptops)

This simple change makes your computer start in 5–15 seconds instead of waiting forever.  
Great for old laptops with hard drives (HDDs) or weak CPUs.

**Before you start – Important!**
- This turns off some CPU security protections to go faster.
- Safe for your own personal laptop (no one else uses it).
- NOT safe for work computers, servers, or if you share the laptop.
- You lose a tiny bit of security against very rare attacks — but for most home users in 2026, it's not a big deal.
- Everything else (graphics, sound, Wi-Fi) stays normal.

### Super Easy Steps

1. **Make a backup** (just in case)  
   Open Terminal and copy-paste this:  
   ```bash
   sudo cp /etc/default/grub /etc/default/grub.backup-$(date +%F)

### Instructions 
Copy-paste this command to open the editor: sudo nano /etc/default/grub

Delete everything inside and paste this instead.

GRUB_DEFAULT=0
GRUB_TIMEOUT=0
GRUB_TIMEOUT_STYLE=menu
GRUB_DISTRIBUTOR="$(lsb_release -is 2>/dev/null || echo Debian)"
GRUB_CMDLINE_LINUX_DEFAULT="quiet loglevel=3 rd.systemd.show_status=false rd.udev.log_level=3 mitigations=off nowatchdog splash"
GRUB_CMDLINE_LINUX="rootwait elevator=deadline"

GRUB_DISABLE_RECOVERY=true
GRUB_DISABLE_OS_PROBER=true
GRUB_DISABLE_SUBMENU=y

GRUB_TERMINAL=console
GRUB_GFXMODE=
GRUB_GFXPAYLOAD_LINUX=
GRUB_BACKGROUND=

### Press Ctrl+O → Enter → Ctrl+X to save and exit.
### Apply and restart

sudo update-grub
sudo reboot

### Your laptop should now boot much quicker — no more black screens, waiting, or charger/lid tricks!
### Extra Speed Tips (Do if you want even faster)

# No Bluetooth? Run this:
sudo systemctl disable --now bluetooth.service

# No need to wait for internet at startup?
sudo systemctl disable --now NetworkManager-wait-online.service

# Turn off crash reporter (not needed on Mint usually)
sudo systemctl mask apport.service

### How to See What's Still Slow (if needed)

# Shows slowest parts
systemd-analyze blame | head -15

# Makes a picture of boot time (open boot.svg in browser)
systemd-analyze plot > boot.svg

### How to Undo (Revert) Everything
sudo mv /etc/default/grub.backup-* /etc/default/grub
sudo update-grub
sudo reboot

That's it!
If it helps a lot, feel free to share this repo with friends who have slow Linux laptops. Questions? Open an issue.
