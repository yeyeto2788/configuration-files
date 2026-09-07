# Linux Tablet

**Description:** Touchscreen calibration for a Goodix-based Linux tablet (rotated
display, needs axis inversion/swap).

## Source / Version

- Type: device
- Firmware/App version: xinput / xorg (unknown versions)
- Last updated: 2026-09-07

## Files

| File | Description |
| ---- | ----------- |
| `99-calibration.conf` | X11 input class calibration for the "Goodix Capacitive TouchScreen". |
| `fix_touch.sh` | Runtime fix applying a coordinate transformation matrix via `xinput`. |

## Backup

`99-calibration.conf` comes from `/usr/share/X11/xorg.conf.d/`; `fix_touch.sh` is a
standalone script kept alongside it.

## Restore

1. Copy `99-calibration.conf` to `/usr/share/X11/xorg.conf.d/99-calibration.conf`
   (`sudo cp 99-calibration.conf /usr/share/X11/xorg.conf.d/`).
2. Restart X or reboot.
3. If the touch mapping is still off after rotation, run `fix_touch.sh` to apply the
   coordinate transformation matrix directly with `xinput`:
   `sh fix_touch.sh`

## Notes

`fix_touch.sh` is a runtime workaround (not persisted); re-run it after each X session
restart if the static calibration in `99-calibration.conf` isn't enough on its own.
