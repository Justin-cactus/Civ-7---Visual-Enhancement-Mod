# Civ-7---Visual-Enhancement-Mod

This is a great project with a lot of meat to it. Let me break it down properly given what I now know about the current state of Civ 7 modding.

---

## 🏛️ Civ 7 Visual Overhaul Mod — Project Breakdown

A few important context notes first:

- As of the June 2025 Update (1.2.2), Civ VII officially launched Steam Workshop support and an initial Modding SDK with FireTuner for debugging. However, this initial SDK version does not include art tools, meaning 3D asset replacement is still off the table for now.
- The UI is written in JavaScript/CSS and is fully moddable. Database files are also unpacked. Art assets are mostly packed, but 2D art (icons, textures) can be edited.
- UI modding workflow involves editing JS/CSS files, and you can check `UI.log` for errors. The in-game console also has a `ReloadUI` command to speed up iteration.

This shapes what's feasible at each tier. Here's your full breakdown:

---

### 🗺️ EPIC 1 — Border & Wall Softening

**Goal:** Reduce the harsh hexagonal geometry of borders and walls by introducing organic curvature and visual softness.

| Story | Scope | Feasibility |
|---|---|---|
| 1.1 Audit border rendering files | Locate the JS/CSS/shader files responsible for drawing city/culture borders | High |
| 1.2 CSS border radius experimentation | Apply `border-radius` or SVG path smoothing to UI-layer border overlays | Medium |
| 1.3 Randomized curve offsets | Add a seeded RNG to introduce subtle, per-hex curvature variation on border edges | Medium |
| 1.4 Wall texture replacement | Swap 2D wall segment textures for softer, hand-painted alternatives | Medium (2D only until art SDK) |
| 1.5 Configurable intensity slider | Let users dial between "geometric" and "organic" softness via a settings panel | Medium |

**Tech surface:** JS/CSS overlays, possible SVG manipulation, 2D texture swaps.

---

### 🌫️ EPIC 2 — Fog of War Restyle

**Goal:** Replace the flat grey cloud cover with something more atmospheric — inspired by Civ 6's aged/parchment aesthetic.

| Story | Scope | Feasibility |
|---|---|---|
| 2.1 Identify fog of war render layer | Locate where fog color/texture is defined (likely a shader or CSS color value) | High |
| 2.2 Sepia/parchment color pass | Replace grey with warm sepia/coffee tones across the fog layer | High |
| 2.3 Texture overlay injection | Layer a noise or parchment texture on top of the fog region | Medium |
| 2.4 Animated fog variation | Add subtle slow animation (drift, fade pulse) to make fog feel alive | Medium |
| 2.5 "Explored but unseen" differentiation | Give the "previously seen" layer a distinct warm-toned treatment vs. fully unexplored | Medium |

**Tech surface:** CSS/shader overrides, 2D texture injection, possibly JS canvas compositing.

---

### 🖥️ EPIC 3 — UI Color Scheme System

**Goal:** Build a flexible theming layer so players can reskin the UI's color palette without deep file edits each time.

| Story | Scope | Feasibility |
|---|---|---|
| 3.1 Audit existing UI CSS variables | Catalog what CSS custom properties (`--color-*`) are already exposed in the game's stylesheets | High |
| 3.2 Build a theme override file | Create a single CSS file that overrides key variables (panel backgrounds, accent colors, text) | High |
| 3.3 Ship 2–3 preset themes | Deliver a "Civ 5 Blue", "Civ 6 Warm", and "Dark Parchment" preset out of the box | Medium |
| 3.4 In-game theme switcher | JS panel in the mod menu to swap themes live without restarting | Medium-Hard |
| 3.5 Custom color picker | Advanced mode: let users specify their own hex values per UI zone | Hard |

**Tech surface:** CSS variable overrides, JS settings panel. This epic has the highest immediate ROI since the UI layer is fully accessible.

---

### 🌄 EPIC 4 — Terrain & Tile Visual Polish

**Goal:** Improve tile rendering quality across zoom levels — crispness when zoomed in, coherence when zoomed out.

| Story | Scope | Feasibility |
|---|---|---|
| 4.1 Audit LOD (Level of Detail) configuration | Find where zoom-level tile rendering thresholds are set | Medium |
| 4.2 Tune LOD transition distances | Adjust at what zoom distance high/medium/low detail tiles swap in | Medium (DB files) |
| 4.3 Sharpen texture filtering settings | Override anisotropic filtering or mipmap settings if exposed | Hard (may need art SDK) |
| 4.4 Tile border contrast boost | Increase edge definition between tile types at mid-zoom via overlay | Medium |
| 4.5 Bird's-eye color grading pass | Add a CSS/shader color grade that kicks in at maximum zoom-out for a more painterly macro view | Medium |

**Tech surface:** Database config files, possible shader/CSS overrides. Partially blocked until art tools are available for the deepest texture work.

---

### 🔧 EPIC 0 — Mod Foundation & Dev Environment

This is the invisible epic that makes everything else possible. Do this first.

| Story | Scope |
|---|---|
| 0.1 Set up SDK + FireTuner | Install the official Modding SDK, enable FireTuner for live debugging |
| 0.2 Enable debug panels | Edit `AppOptions.txt` to set `EnableDebugPanels 1` |
| 0.3 Establish mod folder structure | Create your `.modinfo` file, folder conventions, and a working "hello world" mod |
| 0.4 Set up version control | Git repo from day one — the JS/CSS files are text, so diffs are meaningful |
| 0.5 Study reference mods | Read through Sukritact's Simple UI Adjustments source as a structural reference |

---

### Suggested Attack Order

Given the SDK's current limitations (no art tools yet), a practical first sprint sequence would be:

**Epic 0 → Epic 3 → Epic 2 → Epic 1 → Epic 4**

Epic 3 (UI theming) and Epic 2 (fog of war) are the lowest-friction, highest-impact starting points since they live entirely in the JS/CSS layer that's fully open. Walls/borders (Epic 1) require more creative geometry work, and terrain (Epic 4) has the most blocked stories pending art tool updates.

Want me to help you start scaffolding the mod folder structure and a first working CSS theme override as a proof of concept?
