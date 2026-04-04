# ⚡ quick-linux-boot-tweaks

🛠️ Small boot-time tweaks I’ve used on Debian-based systems to cut down some of the waiting during startup.

💻 Mostly aimed at older laptops, slower drives, or installs that feel more sluggish than they should.

---

## 📌 what this repo is

This repo is just a small set of notes and config ideas for trimming boot delays.

It’s not a magic fix, and it won’t turn every old laptop into a rocket ship. But if your system spends too long sitting at GRUB, waiting on services, or dragging through startup, a few small changes can help.

---

## ⚠️ before changing anything

A couple of these tweaks trade convenience or security for speed.

Things to keep in mind:

- 🔐 `mitigations=off` disables CPU security mitigations
- 🧭 `GRUB_TIMEOUT=0` removes the normal boot menu delay
- 🪟 Disabling OS probing can hide other operating systems from the GRUB menu
- 🧪 Not every machine benefits the same amount

For a personal machine you understand well, that might be an acceptable tradeoff. For shared machines, work systems, or anything sensitive, I’d be more careful.

---

## 🚀 the main GRUB change

First make a backup:

```bash
sudo cp /etc/default/grub /etc/default/grub.backup-$(date +%F)
```

Open the config:

```bash
sudo nano /etc/default/grub
```

Replace the contents with:

```conf
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
```

Save the file, then regenerate GRUB:

```bash
sudo update-grub
```

Reboot to test:

```bash
sudo reboot
```

---

## 🔍 what these changes do

A few quick notes on the important lines:

- ⚡ `GRUB_TIMEOUT=0` skips the usual boot menu countdown
- 🧹 `quiet loglevel=3` reduces console noise during boot
- 🛑 `nowatchdog` can remove a bit of overhead on some systems
- 🔐 `mitigations=off` favors speed over CPU security hardening
- 🚫 `GRUB_DISABLE_OS_PROBER=true` avoids scanning for other OS installs

That last one matters if you dual boot. If you need GRUB to detect Windows or another Linux install, you may want that setting changed instead of copied blindly.

---

## 🧰 extra tweaks

These are optional and depend on what you actually use.

### Bluetooth
If you never use Bluetooth:

```bash
sudo systemctl disable --now bluetooth.service
```

### Network wait
If you don’t need the system to pause waiting for network on boot:

```bash
sudo systemctl disable --now NetworkManager-wait-online.service
```

### Crash reporting
On systems where it exists and you don’t use it:

```bash
sudo systemctl mask apport.service
```

---

## ⏱️ check what is slow

Before changing too much, it helps to look at what is actually taking time.

Show the boot chain summary:

```bash
systemd-analyze
```

Show units that take the longest to initialize:

```bash
systemd-analyze blame | head -15
```

Generate a boot graph:

```bash
systemd-analyze plot > boot.svg
```

Open `boot.svg` in a browser and look for obvious delays.

One useful detail: `systemd-analyze blame` is a rough pointer, not a perfect total of boot time, because some units start in parallel.

---

## 🔁 how to undo it

If you want to go back:

```bash
sudo cp /etc/default/grub.backup-* /etc/default/grub
sudo update-grub
sudo reboot
```

If you disabled services, just re-enable the ones you want:

```bash
sudo systemctl enable --now bluetooth.service
sudo systemctl enable --now NetworkManager-wait-online.service
```

---

## 📝 notes

I’d treat this repo as a starting point, not a one-size-fits-all boot guide.

The best result usually comes from checking what your own machine is waiting on, making one or two changes at a time, and testing properly after each one.
