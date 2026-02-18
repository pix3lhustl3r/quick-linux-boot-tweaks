# quick-linux-boot-tweaks

# Linux Fast Boot Tweaks (especially for Linux Mint & older laptops)

Goal: Reduce boot time from 30–60+ seconds down to ~5–15 seconds on many systems  
(by accepting some trade-offs in security & debuggability).

**Important warnings**  
- `mitigations=off` disables **CPU vulnerability protections** (Spectre, Meltdown, etc.).  
  → Use only on **trusted, single-user** laptops — **not** on shared/multi-user servers.  
- `nowatchdog` disables lockup detection → slightly more performance, but harder to diagnose freezes.  
- These changes prioritize **speed + reliability over nice visuals**.

Tested mostly on Linux Mint 21.x / 22.x and similar Ubuntu derivatives (2023–2026).

### Quick Start (GRUB optimizations)

1. **Backup** your current config
```bash
sudo cp /etc/default/grub /etc/default/grub.bak-$(date +%F)
