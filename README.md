# hp-boot-logo

Replace the HP firmware boot splash (Wolf Security included) with your own image, or restore the HP logo.

This changes the firmware splash shown before Linux starts. It does not change the Omarchy Plymouth theme, BIOS settings, or Sure Start.

## Use

The image can live anywhere. Pass its path:

```bash
hp-boot-logo ~/Pictures/logo.png
```

PNG, JPEG, and other formats ImageMagick can read are fine. Then reboot. The next boot runs a one-shot EFI step, writes the logo, and reboots into Linux. After that, the new splash is shown on every boot.

Restore the HP logo:

```bash
hp-boot-logo --clear
```

Reboot again to apply.

On this machine the command is already linked:

```text
~/.local/bin/hp-boot-logo -> ~/Work/hp-boot-logo/hp-boot-logo
```

`sudo` asks for your password. You do not need to store the image in this repo.

## Image rules

HP only accepts a JPEG, at most 1024×768, at most 32KB. The script converts, shrinks, and compresses for you. A larger source is fine.

The splash is shown centered, not stretched to fill the screen. A small logo stays small. A near-black background is flattened to pure black so it matches the firmware screen. A colored background is left as-is and will show as a rectangle.

## Where files go

| What | Where |
|---|---|
| Your source image | Anywhere you pass on the command line |
| Converted splash | `/boot/EFI/logo/logo.jpg` (written by the script) |
| HP tool, not in git | `efi/CustomLogoApp.efi` in this repo, copied to the EFI partition once |

## First run on a new machine

Needs `sudo`, `magick`, `efibootmgr`, and the EFI partition mounted at `/boot`. Secure Boot must be off.

```bash
sudo pacman -S --needed imagemagick edk2-shell efibootmgr
ln -sf ~/Work/hp-boot-logo/hp-boot-logo ~/.local/bin/hp-boot-logo
```

Copy HP's `CustomLogoApp.efi` to `efi/CustomLogoApp.efi` before the first run. It is not committed. If it is already on `/boot/EFI/logo/`, you can skip that.

The script installs the EFI shell from `edk2-shell` and creates a boot entry that stays at the end of the boot order. Each run only sets it for the next reboot.
