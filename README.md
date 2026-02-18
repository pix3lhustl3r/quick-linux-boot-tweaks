# Fast Boot Tweaks for Linux Mint & Older Laptops

Reduce boot time from 30–60+ seconds down to ~5–15 seconds on many systems  
(especially older laptops, HDDs, low-RAM machines, Linux Mint Cinnamon/XFCE/MATE).

**Important trade-offs**  
- Disables CPU vulnerability mitigations (`mitigations=off`) → faster, but **less secure**  
- Turns off watchdog, verbose logging, graphical GRUB → prioritizes speed & reliability over looks/debugging  
- Best for **single-user trusted laptops**, not servers or shared systems

### One-step GRUB optimization (most effective change)

1. **Backup** your current GRUB config
```bash
sudo cp /etc/default/grub /etc/default/grub.backup-$(date +%F)
