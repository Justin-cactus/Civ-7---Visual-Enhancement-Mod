
## 🏛️ Civ 7 Visual Overhaul Mod — Project Breakdown

A few important context notes first:

- As of the June 2025 Update (1.2.2), Civ VII officially launched Steam Workshop support and an initial Modding SDK with FireTuner for debugging. However, this initial SDK version does not include art tools, meaning 3D asset replacement is still off the table.
- The UI is written in JavaScript/CSS and is fully moddable. Database files are also unpacked, while art assets are mostly packed — though 2D art like icons and textures can still be edited.
- UI modding workflow involves editing JS/CSS files; you can check `UI.log` for errors and use the in-game console's `ReloadUI` command to speed up iteration.

---

### 🔧 EPIC 0 — Mod Foundation & Dev Environment

This invisible epic makes everything else possible. Do this first.

| Story | Scope |
|---|---|
| 0.1 Set up SDK + FireTuner | Install the official Modding SDK, enable FireTuner for live debugging |
| 0.2 Enable debug panels | Edit `AppOptions.txt` to set `EnableDebugPanels 1` |
| 0.3 Establish mod folder structure | Create your `.modinfo` file, folder conventions, and a working "hello world" mod |
| 0.4 Set up version control | Git repo from day one — JS/CSS files are text, so diffs are meaningful |
| 0.5 Study reference mods | Read through Sukritact's Simple UI Adjustments source as a structural reference |

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

### 🌍 EPIC 3 — Terra Incognita Visual Replacement

**Goal:** Replace Civ 7's muddy unexplored-world rendering with a clean, evocative canvas/parchment treatment reminiscent of Civ 6's hand-drawn map aesthetic — and critically, make it visually *distinct* from the Fog of War layer so the two are never confused.

This is closely related to Epic 2 but deserves its own epic because it involves a separate render layer, different thematic goals (mystery vs. obscured-but-known), and potentially its own texture assets.

| Story | Scope | Feasibility |
|---|---|---|
| 3.1 Distinguish Terra Incognita from Fog of War layers | Audit the rendering pipeline to confirm these are separate layers and identify each one's hook points | High |
| 3.2 Replace Terra Incognita base color | Change the flat brownish void to a warm cream/linen canvas tone as a first pass | High |
| 3.3 Apply canvas/parchment texture overlay | Inject a hand-drawn canvas texture (blank aged paper, visible fiber grain) over the unknown region | Medium |
| 3.4 Add subtle cartographic detail | Overlay faint decorative elements (compass rose hints, stylized wave patterns on unknown oceans, "Here Be Dragons" style vignette at the edges) | Medium |
| 3.5 Edge feathering & transition | Soften the hard boundary between the known world and the Terra Incognita region with a painterly fade | Medium |
| 3.6 Tile-reveal animation | When a tile is first explored, animate it "painting in" from the canvas state rather than popping in abruptly | Hard |
| 3.7 Selectable Terra Incognita styles | Offer at least two presets: "Civ 6 Parchment Canvas" and a darker "Antique Black" option for those who prefer the classic look | Medium |

**Tech surface:** 2D texture injection, CSS/shader layer overrides, JS animation hooks. The thematic complexity here (it needs to feel like an undiscovered world, not just a greyed-out one) makes this one of the most artistically interesting epics in the project.

**Important nuance:** Epics 2 and 3 share a color family (warm sepia tones) but must be kept visually separable. The design contract should be: *Fog of War = darkened/obscured known world; Terra Incognita = blank canvas waiting to be painted.* Consider establishing this as a written style guide before implementing either.

---

### 🖥️ EPIC 4 — UI Color Scheme System

**Goal:** Build a flexible theming layer so players can reskin the UI's color palette without deep file edits each time.

| Story | Scope | Feasibility |
|---|---|---|
| 4.1 Audit existing UI CSS variables | Catalog what CSS custom properties (`--color-*`) are already exposed in the game's stylesheets | High |
| 4.2 Build a theme override file | Create a single CSS file that overrides key variables (panel backgrounds, accent colors, text) | High |
| 4.3 Ship 2–3 preset themes | Deliver a "Civ 5 Blue", "Civ 6 Warm", and "Dark Parchment" preset out of the box | Medium |
| 4.4 In-game theme switcher | JS panel in the mod menu to swap themes live without restarting | Medium-Hard |
| 4.5 Custom color picker | Advanced mode: let users specify their own hex values per UI zone | Hard |

**Tech surface:** CSS variable overrides, JS settings panel. Highest immediate ROI since the UI layer is fully accessible.

---

### 🌄 EPIC 5 — Terrain & Tile Visual Polish

**Goal:** Improve tile rendering quality across zoom levels — crispness when zoomed in, coherence when zoomed out.

| Story | Scope | Feasibility |
|---|---|---|
| 5.1 Audit LOD (Level of Detail) configuration | Find where zoom-level tile rendering thresholds are set | Medium |
| 5.2 Tune LOD transition distances | Adjust at what zoom distance high/medium/low detail tiles swap in | Medium (DB files) |
| 5.3 Sharpen texture filtering settings | Override anisotropic filtering or mipmap settings if exposed | Hard (may need art SDK) |
| 5.4 Tile border contrast boost | Increase edge definition between tile types at mid-zoom via overlay | Medium |
| 5.5 Bird's-eye color grading pass | Add a CSS/shader color grade that kicks in at maximum zoom-out for a more painterly macro view | Medium |

**Tech surface:** Database config files, possible shader/CSS overrides. Partially blocked until art tools ship for the deepest texture work.

---

### Suggested Attack Order

Given the SDK's current art tool limitations, a practical sprint sequence is:

**Epic 0 → Epic 4 → Epic 2 → Epic 3 → Epic 1 → Epic 5**

Epics 4 (UI theming) and 2 (fog of war) are lowest friction and highest impact since they live entirely in the open JS/CSS layer. Epic 3 (Terra Incognita) follows naturally since it shares the same render investigation work as Epic 2 and the two should be designed in tandem. Borders/walls (Epic 1) require more creative geometry work, and terrain (Epic 5) has the most stories blocked pending art tools.

One final note: Epics 2 and 3 together are essentially your **"cartographic soul"** rework — the two things that define the emotional feeling of looking at an undiscovered world. Getting those right first will give the entire mod its identity.
