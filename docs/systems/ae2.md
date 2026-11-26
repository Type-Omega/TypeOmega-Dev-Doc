# AE2 in Type Omega

Type Omega extends Applied Energistics 2 with higher-tier interfaces, faster acceleration cards and automatic pattern upload, all implemented from scratch in TypeOmega Core (D-442).

## Tiered ME Interfaces

**ME Interface (II)–(V)** expand on the plain AE2 ME Interface (tier 0). Every tier above the plain interface has **36 config slots** and **36 storage slots**, and is available as both a full block (`me_interface_ii`–`v`) and a cable part (`cable_me_interface_ii`–`v`).

| Tier | Slot multiplier | Recipe circuit |
| --- | --- | --- |
| II | ×4,096 | MV |
| III | ×262,144 | EV |
| IV | ×16,777,216 | LuV |
| V | ×1,073,741,824 | UV |

**In-place upgrade:** Sneak + right-clicking a lower-tier interface (including the plain AE2 interface) with a higher-tier one of the same form (block on block, part on part) replaces it in place. Configuration, stored contents, installed upgrade cards, priority and custom name are preserved, and the old interface is returned to the player.

## Tiered Acceleration Cards

**Acceleration Card (II)–(V)** are crafted from four plain AE2 Acceleration Cards around a circuit of the tier's voltage. Each tier is worth 8× the previous one, starting from the plain AE2 card (×1).

| Tier | Equivalent AE2 cards | Recipe circuit |
| --- | --- | --- |
| II | ×8 | EV |
| III | ×64 | LuV |
| IV | ×512 | UV |
| V | ×4,096 | UEV |

**Where they apply:** Tiered Acceleration Cards speed up AE2 and ExtendedAE transfer machines:

- **Import and Export Buses** (operations per tick)
- **ME IO Port** (3 card slots) and **Extended IO Port** (5 card slots, items moved per tick)

The machine's card slot limit is shared across all tiers, counting the highest tiers first. Each installed card contributes its multiplier toward the machine's scaling table (`table(cards) × (sum of multipliers) / cards`).

## Automatic Pattern Upload

When encoding a processing pattern after transferring a recipe from EMI or JEI, the encoded pattern is automatically uploaded to the matching **ME Pattern Provider** (or Extended Pattern Provider) on the same ME network instead of waiting in the terminal:

- Works in both **AE2's Pattern Encoding Terminal** and **AE2 WCWT** (AE2 Wireless Comprehensive Work Terminal).
- Matches the recipe type/category name against active pattern providers on the grid. An exact name match beats a partial substring match, and the matching provider name must be unique (otherwise the terminal reports no match or ambiguity).
- Duplicate patterns already present in the target provider are skipped.
- Hold **Shift** while pressing Encode to keep the pattern in the terminal instead of uploading it.

!!! note "ExtendedAE Plus compatibility"
    Automatic pattern upload is reimplemented from [ExtendedAE Plus](https://github.com/GaLicn/ExtendedAEPlus) (LGPL-3.0, credited in `THIRD_PARTY_LICENSES.md`) without requiring it in the pack. If ExtendedAE Plus is installed, TypeOmega Core automatically disables its own upload hook so patterns are not sent twice.

## Pack AE2 Mods

Alongside AE2 and ExtendedAE, the pack includes:

- **AE2 Wireless Terminals** (`applied-energistics-2-wireless-terminals`)
- **AE2 WCWT** (`ae2-wcwt`, MIT) — the wireless crafting/work terminal used in the pack
- **ME Placement Tool**
- **ME Requestable Input Bus/Hatch** and the **wireless ME network** (D-442, implemented in TypeOmega Core)
