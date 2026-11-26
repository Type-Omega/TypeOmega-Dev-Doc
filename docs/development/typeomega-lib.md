# TypeOmega Lib

[TypeOmega Lib](https://github.com/Type-Omega/TypeOmega-Lib) (`com.typeomega:typeomega-lib`) is the shared library used across Type Omega mods. Below are the client and testing utilities added in 0.8.1–0.8.4.

## GTNH-Style Paginated Tooltips

Package: `com.typeomega.lib.tooltip.paging` (`TooltipPaging`, `TooltipPager`, `EmiModLine`).

Long item tooltips are split into pages when they exceed the available screen height at the current GUI scale **or** exceed **20 body rows**, even on tall screens:

- **Whole sections per page:** `TooltipPager` breaks pages at separators and section headers (`softBreakBefore`) and spreads lines as evenly as possible across the fewest pages. A section is only split if that section alone is taller than one page.
- **Header:** Line 0 (the item name) repeats at the top of every page.
- **Navigation:** Press **Tab** (`Shift+Tab` goes back) or use **Ctrl + mouse wheel**.
- **EMI integration:** Paging runs in `ItemTooltipEvent` on the render thread when a screen is open, so tooltips hovered in EMI's item list are paged while EMI's background search index still sees all lines. `EmiModLine` automatically drops our trailing italic blue mod-name line when EMI is configured to append its own (`appendModId` and `appendItemModId`), avoiding duplicate mod names.

To opt a mod's items in during client setup (already enabled by default for `typeomega`, `typeomegalib`, `gtna` and `agronomica`):

```java
TooltipPaging.enable("mymod");
```

## UI Module

Package: `com.typeomega.lib.ui`.

- **`FlowArrowWidget`:** Animates arrows or pipes painted directly in the panel's pixel-art texture without modifying the art asset. Any layout mark whose id starts with `flow_` (`flow_right`, `flow_left`, `flow_down`, `flow_up`) turns white pixels (`0xFFFFFFFF`) inside the mark into a grey-to-white fill cycling every 2000 ms.
- **`InfoLinesWidget`:** Draws recipe and machine info lines. When a line is wider than the panel width, it is automatically scaled down (`scale = room / width`) so text never overflows the frame.
- **Unused slot removal (`IllustratedRecipeUI`):** When building a recipe page, `IllustratedRecipeUI.removeUnusedSlots` removes any item or fluid slots defined in the panel layout that the current recipe does not use, showing only active inputs and outputs.

## Shared HUD Editor

Package: `com.typeomega.lib.ui.hud` (`MoveableHud`, `HudRegistry`, `HudEditorScreen`, `HudPositionStore`, `HudClient`).

All mods register their overlays in one shared registry so the player uses a single keybind (**H** by default, under the `TypeOmega Lib` key category) to open `HudEditorScreen`, toggle HUDs on or off, and drag them to persisted screen-percentage positions (`0..100%`) stored in `config/typeomegalib-huds.json`.

To register a HUD during client setup:

```java
HudRegistry.register(new MoveableHud() {
    @Override public String id() { return "mymod:my_hud"; }
    @Override public Component displayName() { return Component.translatable("hud.mymod.my_hud"); }
    @Override public int width() { return 140; }
    @Override public int height() { return 48; }
    @Override public boolean isEnabled() { return MyConfig.hudEnabled; }
    @Override public void setEnabled(boolean enabled) { MyConfig.setHudEnabled(enabled); }
    @Override public void render(GuiGraphics graphics, int x, int y, int width, int height) {
        // draw HUD contents at (x, y)
    }
});
```

Optional overrides on `MoveableHud`:

- `shouldRenderInGame()` — extra visibility condition on top of `isEnabled()` (ignored in `HudEditorScreen` so enabled HUDs can still be positioned before data arrives).
- `defaultXPercent()` / `defaultYPercent()` — initial anchor percentages before the player moves the HUD (default `5`, `5`).

## GameTest Filtering

See [GameTests](gametests.md#filtering-gametests) for `-Ptests=...`, `TestBatches.HEAVY`, and `@HeavyTest`.
