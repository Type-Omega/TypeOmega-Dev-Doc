# Kardashev Meter

!!! note "In development"
    The Kardashev Meter rework (D-443) is currently in development. This page documents the approved design.

The **Kardashev Meter** (D-443) measures a team's total power generation on the Kardashev scale.

## Block and Screen

- **Single block:** Stays a single block in the first chapter with its existing recipe.
- **Dedicated screen:** Opens a UI with three tabs — **Summary**, **History** and **Sources** — plus a **HUD toggle** (similar to the Flux Matrix).
- **Scale and readouts:**
  - The official **K** value uses the per-tier table from D-380 (1 A of each type's first tier is the integer: EV = 1, ZPM = 2, UEV = 3, OpV = 4; 8 EU/t = 0; linear in `log4(EU/t)` in between; 10-minute average, megastructure ceiling, tracked per team).
  - The screen also shows total power in **watts** and **Sagan's K** as an extra line.
- **History:** Keeps **6 hours** of history.

## Counted Sources

Power generation is aggregated across:

- Wireless energy networks
- Boilers
- Local GTCEu generators (including the Tokamak)

Power flowing through **Wireless Dynamo Hatches** is not double-counted.

## HUD

The Kardashev Meter HUD uses TypeOmega Lib's [shared HUD editor](../development/typeomega-lib.md#shared-hud-editor), so all mod HUDs share one drag editor, one keybind and one position config file.
