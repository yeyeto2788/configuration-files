# PocketCHIP Screen Calibration

**Description:** Touchscreen calibration config for the PocketCHIP.

## Source / Version

- Type: device
- Firmware/App version: xinput-calibrator (Debian package, unknown version)
- Last updated: 2026-09-07

## Files

| File | Description |
| ---- | ----------- |
| `99-calibration.conf` | X11 input class calibration for the PocketCHIP touchscreen. |

## Backup

Copy `/usr/share/X11/xorg.conf.d/99-calibration.conf` from the device.

## Restore

1. Install the calibrator: `sudo apt-get install xinput-calibrator`
2. Create/overwrite `/usr/share/X11/xorg.conf.d/99-calibration.conf` with the file from
   this folder: `sudo nano /usr/share/X11/xorg.conf.d/99-calibration.conf`
3. Reboot the PocketCHIP: `sudo shutdown -h now`

## Notes

N/A
