# Kirimoto

**Description:** [Kiri:Moto](https://grid.space/kiri/) CAM tool library and cut profiles
for CNC routing, tuned as a conservative starting point for a 500W ER11 spindle on
NEMA17-driven lead screws (the [Feungsake CNC](../../hardware/machines/cnc/feungsake_cnc/README.md),
1000x1000x130mm work area).

## Source / Version

- Type: software
- Firmware/App version: Kiri:Moto 4.7.3
- Last updated: 2026-09-17

## Files

| File | Description |
| ---- | ----------- |
| `tools.km` | Tool library: the stock imperial set plus 4 added metric endmills. |
| `default_cut_profile.km` | Baseline conservative CAM process profile (all other profiles clone this). |
| `ply_cut_profile.km` | Cut profile guesstimated for plywood. |
| `mdf_cut_profile.km` | Cut profile guesstimated for MDF. |
| `foam_cut_profile.km` | Cut profile guesstimated for foam (XPS/EVA/polystyrene). |
| `acrylic_cut_profile.km` | Cut profile guesstimated for acrylic. |
| `test_coupon.svg` | Small (90x60mm) importable test panel — outline, pocket, slot, and two calibration holes — for scrap-testing a profile before trusting it on a real project. |

## Backup

Exported from Kiri:Moto: **Tools** panel → Export (for `tools.km`), and **Process** panel
→ Export (for each `*_cut_profile.km`). Each `.km` file is just base64-encoded JSON —
decode with `Buffer.from(fs.readFileSync(f,'utf8').trim(),'base64').toString('utf8')` in
Node (or the `base64` CLI) if you need to inspect/edit one directly.

## Restore

1. Open Kiri:Moto (`grid.space/kiri` or a local install) in CAM mode.
2. **Tools** panel → Import → `tools.km`.
3. **Process** panel → Import → whichever `*_cut_profile.km` matches the material you're
   about to cut.
4. **File** → Import → `test_coupon.svg` if you want to run a scrap test first (see
   below).

## Notes

### Tool library (`tools.km`)

The original file had 7 imperial tools (1/4", 1/8", 1/16" endmills, a 1/8" vee bit, a
1/8" ballmill, 1/8" and 1/4" drills). Added 4 metric double-flute endmills, shank
diameter = flute diameter (no reduced neck):

| Name | Diameter | Flute length | Type |
| ---- | -------- | ------------ | ---- |
| `end 3mm 2fl up` | 3mm | 17mm | upcut |
| `end 3mm 2fl down` | 3mm | 17mm | downcut |
| `end 6mm 2fl up` | 6mm | 22mm | upcut |
| `end 6mm 2fl down` | 6mm | 22mm | downcut |

Kiri:Moto's tool schema has no dedicated up/down-cut field — cut direction is only
reflected in the name and doesn't change how Kiri:Moto generates toolpaths. It only
matters for which physical bit you chuck up when a profile calls for "up" or "down".

Also added a full metric twist-drill set, 2mm through 10mm in 1mm steps. Flute/shank
lengths are approximate DIN 338 jobber-length drill dimensions (a reasonable stand-in if
your actual bits differ — Kiri:Moto mainly uses these for clearance/visualization, not as
a hard machining constraint):

| Name | Flute length | Shank length |
| ---- | ------------ | ------------ |
| `drill 2mm` | 24mm | 25mm |
| `drill 3mm` | 33mm | 28mm |
| `drill 4mm` | 43mm | 32mm |
| `drill 5mm` | 52mm | 34mm |
| `drill 6mm` | 57mm | 36mm |
| `drill 7mm` | 66mm | 43mm |
| `drill 8mm` | 75mm | 42mm |
| `drill 9mm` | 80mm | 45mm |
| `drill 10mm` | 87mm | 46mm |

Also added 3 V-bits, 3.175mm (1/8") shank, sharp point (`taper_tip: 0`):

| Name | Included angle | Stored `taper_angle` | `flute_len` |
| ---- | --------------- | --------------------- | ----------- |
| `vee 3.175mm 30deg` | 30° | 15° | 5.925mm |
| `vee 3.175mm 45deg` | 45° | 22.5° | 3.833mm |
| `vee 3.175mm 60deg` | 60° | 30° | 2.750mm |

Kiri:Moto's `taper_angle` field is **not** the marketed included angle — it's the
half-angle from the tool's centerline, computed internally as
`atan((flute_diam - taper_tip) / 2 / flute_len)` (confirmed from
[Kiri:Moto's source](https://github.com/GridSpace/grid-apps), `src/kiri/mode/cam/core/tool.js`).
So a "60° V-bit" as sold needs `taper_angle: 30`. `flute_len` was solved from that same
formula so each bit renders at the correct real-world angle rather than a guessed value.

**Fixed:** the pre-existing `vee 1/8` tool (id 1003, tool #7 in Kiri:Moto's list — it
sorts by the `order` field, not import order) had `metric: true` but stored
imperial-looking numbers (`flute_diam: 0.125`, `flute_len: 1.5`). Kiri:Moto takes those
numbers at face value when `metric` is true, so it was rendering as a 0.125**mm** taper
mill instead of a 1/8**"** one. Flipped `metric` to `false`; the dimension numbers were
already correct for inches, so nothing else needed to change (`taper_angle` is a unitless
ratio — see the V-bit note above — so it stayed valid either way).

### Mill direction

`camMillDirection` is set to `conventional` (not `climb`) on every profile. Climb milling
gives a cleaner edge finish but bites harder into the material and is more likely to
grab/stall a machine that's short on rigidity or torque — safer default here.

### Material cut profiles

All four material profiles are clones of `default_cut_profile.km` with spindle RPM,
feed/plunge, depth-per-pass, stepover, and tool assignment overridden per material.
Roughing operations (Area/Rough/Pocket/Helical) use the 6mm upcut bit for faster
clearing and better chip evacuation; finishing operations (Contour/Outline/Trace/Level)
use the 3mm bit, downcut where a clean top edge matters more than chip clearance.

| Material | Spindle | Rough feed / DOC / stepover | Finish feed / DOC / stepover | Plunge | Finish tool | Why |
| -------- | ------- | ---------------------------- | ------------------------------ | ------ | ----------- | --- |
| Plywood | 10,000 RPM | 500mm/min / 1.5mm / 30% | 400mm/min / 1.5mm / 25% | 100mm/min | 3mm downcut | Downcut pulls chips down instead of up, reducing splintering on the top ply. |
| MDF | 12,000 RPM | 600mm/min / 2mm / 35% | 450mm/min / 1.5mm / 25% | 100mm/min | 3mm upcut | Highest RPM of the set — MDF is abrasive/dusty, steady chip load at higher RPM avoids rubbing/burning through the dust. Upcut throughout for chip evacuation. |
| Foam | 8,000 RPM | 1200mm/min / 5mm / 50% | 900mm/min / 3mm / 30% | 300mm/min | 3mm upcut | Almost no cutting resistance, so feed/DOC go *up*, not down. RPM is dropped instead — foam melts from frictional heat more than it resists cutting force. |
| Acrylic | 10,000 RPM | 400mm/min / 1mm / 25% | 350mm/min / 1mm / 20% | 80mm/min | 3mm downcut | Most conservative of the four — shallow passes and gentle plunge to avoid melting/re-welding chips to the edge. A 2-flute bit isn't ideal for acrylic (a single/O-flute bit does much better); downcut at least helps the top edge. |

Rapids (`camFastFeed` 1500mm/min XY, `camFastFeedZ` 400mm/min) and Z-clearance are
inherited unchanged from `default_cut_profile.km` across every material profile.

Three more settings, applied to all five profiles (the base + 4 materials):

- **`camEaseAngle`** lowered from 10° to 5° — a shallower ramp into pockets spreads the
  plunge load over more distance, easier on a 500W spindle and on the bit tip.
- **`camRoughStock`/`camRoughStockZ`** (stock left behind by a rough pass for a
  subsequent finishing pass to clean up) set to **0.2mm** for plywood/MDF/foam, **0.1mm**
  for acrylic — a thin allowance so a separate Outline/Contour finishing pass at full
  depth removes just that skin instead of both passes cutting to the same line. Improves
  edge quality and reduces deflection on the final pass; acrylic gets a thinner allowance
  since less material engagement means less frictional heat, and heat is what causes
  melting/re-welding on that material specifically.
- **`camTabsHeight`/`camTabsWidth`** (hold-down tab size): plywood/MDF keep the original
  2mm/5mm tabs; foam and acrylic are dropped to 1mm/3mm — foam barely needs holding at
  all, and acrylic is brittle enough that smaller tabs mean less force (and less
  chip-out risk) when snapping the part free.

### Test coupon (`test_coupon.svg`)

A small 90x60mm panel with 5 shapes, meant to be cut once per material/profile on scrap
before trusting the profile on a real part:

| Shape (SVG id) | Suggested Kiri:Moto op | Tests |
| -------------- | ----------------------- | ----- |
| `outline` (rounded rect, whole panel) | Outline | Finish feed/edge quality, frees the coupon from the stock |
| `pocket` (rect, top-left) | Pocket / Area | Roughing feed, stepover, depth-per-pass |
| `slot` (thin rect, top-right) | Outline / Trace | Chip evacuation and bit deflection in a narrow feature |
| `hole-3mm` (circle) | Outline / Drill, with the 3mm bit | Dimensional accuracy of a small hole |
| `hole-6mm` (circle) | Outline / Drill, with the 6mm bit | Dimensional accuracy of a larger hole |

At 90x60mm it fits comfortably on both known machines in this repo (even the smaller
[T8 CNC](../../hardware/machines/cnc/t8_cnc/README.md)'s 100x150mm bed), so it's a safe
default regardless of which machine you end up running it on.

The two calibration holes are encoded as `<path>` arcs rather than native `<circle>`
elements — some SVG importers (Kiri:Moto's included) only reliably parse `<path>`
geometry, and a bare `<circle>` can come through malformed or get dropped. Two
semicircular arcs (`A r,r 0 1,0 ...`) is the standard, broadly-compatible way to encode a
circle in an SVG meant for CAM/laser import.

### Cleaning up old presets/tools in the Kiri:Moto app

Devices, Process profiles, and Tools all live in the browser's local storage, managed
from three panels — keyboard shortcuts `e` (Devices), `l` (Process/settings), `o`
(Tools), or the equivalent gear-icon menu, when focus isn't in a text field:

- **Process profiles**: every saved profile (except `default`, which can't be deleted)
  shows as a row with 4 icons — edit (raw JSON), name = load, export, and a trash icon to
  delete it permanently.
- **Tools**: one flat list, not separate "libraries". Pick a tool from the dropdown, hit
  **Delete**, then **Save** to persist.
- **Devices**: same select-then-delete pattern as Process.

**Import gotcha**: re-importing a `tools.km` does **not** replace your existing tools —
it only *adds* tools whose `id` isn't already present in your browser's local list, and
silently skips any id that's already there. So if you'd previously imported an older
(e.g. buggy) copy of `tools.km`, re-importing a fixed one won't update that tool in-app —
you have to either delete the stale tool first (Tools panel → select → Delete → Save)
then import again, or just fix the field by hand in the tool's edit form. Process profile
imports behave differently and *do* replace cleanly on confirm ("Replace process X?").

**Nuclear option — confirmed working (2026-09-17)**: `Shift+Z` → "clear all settings and
preferences?" wipes *everything* (all devices, all process profiles, the entire tool
list, current workspace) and reloads. This is the reliable way to clear out stale
imported tools/profiles that the merge-by-id import behavior above would otherwise leave
behind. Export anything you want to keep first — there's no undo. After a reset, make
sure you're importing the current file from this folder and not an older local download,
since the reset alone won't fix a stale file being re-imported.

### Caveats — these are guesstimates, not measured values

Every number above (spindle RPM, feed, plunge, depth-per-pass, stepover, rapids) is a
starting-point guess based on general small-endmill feeds/speeds guidance for a 500W
spindle — **none of it has been validated against the Feungsake CNC's actual GRBL
limits**, because [`feungsake_cnc/README.md`](../../hardware/machines/cnc/feungsake_cnc/README.md)
is still a placeholder with no `$$` settings dump.

This matters more than usual here because the Feungsake CNC is lead-screw driven on all
3 axes across a 1000x1000mm gantry with NEMA17 motors — long unsupported lead screws are
prone to whip/resonance at higher RPM, and NEMA17 has less torque margin than the larger
motors this frame size would normally pair with. If anything, that argues for staying
conservative rather than pushing speeds up, until real numbers are on hand.

**Before trusting these on a real project:**

1. Dump the Feungsake CNC's GRBL settings (`$$`, `$#`, `$G`, `$I`, `$N` over serial,
   same process documented in the T8 CNC README) and record them in
   `feungsake_cnc/README.md`.
2. Compare `$110`/`$111`/`$112` (max rate) and `$120`-`$122` (acceleration) against the
   feed/plunge/rapid numbers here — GRBL silently clamps any commanded feed above
   `$110-112`, so if the real max rate is lower than what a profile requests, the machine
   just runs slower than Kiri:Moto's time estimate, but if it's *much* lower it may be
   worth re-tuning the profile numbers down to match reality.
3. Run `test_coupon.svg` on scrap of each material before cutting real stock. Watch/listen
   for stalled steps (grinding/skipping sound, position drift), chatter, or melting
   (acrylic/foam), and back off feed, RPM, or depth-per-pass from there rather than
   trusting the guesstimate outright.
