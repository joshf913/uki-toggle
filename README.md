# uki-toggle
uki-toggle is a bash script i maaaaade for Arch that toggles between UKIs (Unified Kernel Images) and **traditional kernel+initramfs** booting with GRUB.

It automatically detects your bootloader, initramfs generator, and your UKI directory. Hopefully no manual config needed.

---

## y?
- **UKI mode** bundles the kernel, initramfs, and the cmdline into a single .efi file, which is useful for Secure Boot, TPM encryption (of which two things I dont care for), faster boot times (at least for me), and a splash screen that doesn't need plymouth to work.
- **Traditional mode** uses separate 'vmlinuz' and 'initramfs' files to boot, which is more flexible and simpler.

This script lets you switch betweeeeeeeeeeeeeeen those two modes quickly, without touching configs manually. I found it annoying so I made it automated. I guess Linus was right.

---

## Features!
- Toggle UKI / Traditional mode with a single command.
- Automatically detects GRUB config path ('/boot/grub/grub.cfg', etc)
- Automatically detects UKI directories. ('/boot/EFI/Linux', '/efi/EFI/Linux', etc)
- Works with all installed kernels (as long as they are prefaced by 'linux')
- Automation with -y / --yes flags.
- Included bash completion file (subcommands only, no flags.)
- It is reversible and does the least amount of changes it can to avoid issues.
- No hardcoding, cause I like automation.

---

## Requiiirements (in a british accent imagine that in a british accent!!)
- Arch Linux (or derivative) with:
  - **GRUB** as the bootloader.
  - **mkinitcpio** as the initramfs generator.
  - **systemd-ukify** which is part of the systemd package (yes I also hate SystemD but I literally just moved to Linux).
- Root privileges (as the script requires root or sudo to modify certain files).

---

## Installation -todo

---

# Usage
```
uki-toggle [COMMAND] [OPTIONS]
```
### Commands

| Command          | Short | Description |
| :--------------- | :---- | :---------- |
| `enable`         | `-e`  | Switch to UKI boot mode |
| `disable`        | `-d`  | Switch to traditional boot mode |
| `toggle`         | `-t`  | Toggle between modes |
| `status`         | `-s`  | Show current mode and system info |
| `reload`         | `-r`  | Rebuild initramfs/UKIs and regenerate GRUB (no mode change) |
| `help`           | `-h`  | Show this help |
| `version`        | `-v`  | Show version |

### Options

| Option          | Description |
| :-------------- | :---------- |
| `-y`, `--yes`   | Automatically answer 'yes' to any prompt (for automation) |

### Examples

```bash
# Enable UKI mode
uki-toggle enable

# Short form
uki-toggle -e

# Enable UKI mode, skip confirmation (for scripts)
uki-toggle -y enable

# Switch back to traditional
uki-toggle disable

# Toggle between modes
uki-toggle toggle

# Rebuild after kernel update or config change
uki-toggle reload

# Check current status
uki-toggle status
```

---

## How It Works
**UKI mode**:
   - Uncomments `default_uki` and `default_options` lines in `/etc/mkinitcpio.d/linux*.preset`
   - Makes `/etc/grub.d/15_uki` executable (detects `.efi` files)
   - Makes `/etc/grub.d/10_linux` non-executable (hides traditional entries)
   - Runs `mkinitcpio -P` to build UKIs
   - Runs `grub-mkconfig` to regenerate GRUB menu

**Traditional mode**:
   - Comments out those same lines
   - Makes `10_linux` executable, `15_uki` non-executable
   - Rebuilds initramfs only, regenerates GRUB

All changes are fully reversible and you can switch back anytime.

---

## License
```
Copyright (C) 2026  Miaoumi (me)
This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published by
the Free Software Foundation.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
```

---

## Contributing
i doubt anyoen will contribute but if u want to go ahead ill look at it
