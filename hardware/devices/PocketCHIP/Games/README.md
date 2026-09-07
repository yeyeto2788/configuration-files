# Emulating Games on the PocketCHIP

**Description:** mednafen emulator configuration and a Zenity-based launcher GUI for
running retro game ROMs on the PocketCHIP.

## Source / Version

- Type: device
- Firmware/App version: mednafen (Debian package, unknown version)
- Last updated: 2026-09-07

## Files

| File | Description |
| ---- | ----------- |
| `mednafen-09x.cfg` | mednafen emulator config (video/sound drivers, per-system scaling). |
| `medGUI.sh` | Zenity GUI script to pick and launch a ROM. |

## Backup

`mednafen`'s config lives at `/home/chip/.mednafen/mednafen.cfg` on the device; the
launcher script is wherever it was placed (e.g. the Desktop).

## Restore

1. Install mednafen: `sudo apt-get install mednafen libsdl2-dev`
2. Run `mednafen` once so it generates `/home/chip/.mednafen/mednafen.cfg`, then replace
   it with `mednafen-09x.cfg` from this folder (or hand-apply the changes below):

   | Original line | Changed line |
   | ------------- | ------------ |
   | `video.driver opengl` | `video.driver sdl` |
   | `sound.device default` | `sound.device sexyal-literal-default` |
   | *(added)* | `gba.xscalefs 2.000000` |
   | *(added)* | `gba.yscalefs 2.000000` |
   | *(added)* | `gba.stretch full` |
   | *(added)* | `gba.yres 272` |
   | *(added)* | `gba.xres 480` |
   | *(added)* | `gb.stretch aspect` |
   | *(added)* | `gb.xres 480` |
   | *(added)* | `gb.xscalefs 2.000000` |
   | *(added)* | `gb.yres 272` |
   | *(added)* | `gb.yscalefs 2.000000` |
   | *(added)* | `snes.stretch full` |
   | *(added)* | `snes.xres 480` |
   | *(added)* | `snes.xscalefs 2.000000` |
   | *(added)* | `snes.yres 272` |
   | *(added)* | `snes.yscalefs 2.000000` |
   | *(added)* | `nes.stretch aspect` |
   | *(added)* | `nes.xres 480` |
   | *(added)* | `nes.xscalefs 2.000000` |
   | *(added)* | `nes.yres 272` |
   | *(added)* | `nes.yscalefs 2.000000` |

3. To launch a game directly: `mednafen -fs 1 /path/to/rom`
4. For the GUI launcher: `sudo apt-get install zenity`, copy `medGUI.sh` to the device,
   `chmod +x medGUI.sh`. Edit the `/home/chip/roms` path inside the script if ROMs live
   elsewhere. Optionally place it on the Desktop for quick access.

## Notes

- `alt+shift+1` opens the controller configuration window in mednafen.
