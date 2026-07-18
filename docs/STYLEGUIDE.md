# Style Guide — YubiKey Engraver (for AI contributors)

> Read this **before** editing `yubikey-engraver.html`. Every rule below is
> grounded in the existing file — match it, don't invent new conventions.

Audience: an AI agent (or human) making changes. The goal is that any edit is
indistinguishable in style from the original author's, and — more importantly —
that it never silently breaks laser output.

---

## 1. Load-bearing invariants (read first; never break these)

### 1a. Single-file invariant
The entire app is **one file**: `yubikey-engraver.html`. No build step, no
bundler, no `npm`, no framework, no imports, no split CSS/JS files. It runs
from `file://` or any static host (including GitHub Pages).

- Do not extract CSS/JS into separate files.
- Do not introduce a build tool, package manager, transpiler, or TypeScript.
- The only external resource allowed is **Google Fonts** (already preconnected
  in `<head>`). Do not add CDNs, trackers, or other font providers.

### 1b. Laser-output invariants (these wreck physical engravings if violated)
- **All model dimensions are in millimetres (mm).** Never px, never cm. The
  export pipeline multiplies mm × px-per-mm to produce a canvas at true
  physical size.
- **Export polarity is `black = engrave, white = skip`.** The exported PNG is
  the engraving mask, not a pretty picture. Do not invert this, do not add
  colour to the export path.
- **Exported PNG must embed physical DPI** (the `pHYs` chunk) so LightBurn /
  LaserGRBL land it at real size. Any new export path must do the same.
- **The clip mask is even-odd**: body-inset **minus** the keyring hole, and on
  the **front** side also minus the touch pad. Art outside the mask does not
  engrave. Preserve this topology.

If a change touches export, scale, or masking — re-read section 4 before
proceeding and verify the output is still physically correct.

---

## 2. File anatomy (preserve this section order)

```text
<!DOCTYPE html>
/* header comment: app name, author, feature bullet list — keep it accurate */
<html>
  <head>
    <meta …>                      viewport + mobile web-app metas + theme-color
    <title>
    <link>                        Google Fonts (Space Grotesk + IBM Plex Mono)
    <style>                       ALL CSS here, in this order:
      :root { design tokens }
      reset (* { box-sizing… })
      layout (#app grid, header, .col)
      components (.sec, .lib, .row, .btnrow, .layers, .exportBtn…)
      responsive (@media max-width:980px flex fallback)
  <body>
    #app                          display:grid, 3 columns + header row
      header                      model select + FRONT/BACK tabs
      aside.col.left              library, upload, text, layers
      main.center                 editor canvas + engraved previews
      aside.col.right             properties, mask, export, sticker, project
    <script>                      ALL JS here, fenced into named sections
```

JavaScript is organised under **comment banners** — two styles, use both as the
file does:

```js
/* =====================================================================
   SECTION TITLE — one-line description
   ===================================================================== */
```
```js
/* ---------- sub-section name ---------- */
```

When adding code, put it under the correct banner. Add a new banner if you
introduce a new concern; don't drop code between banners untitled.

---

## 3. HTML conventions

- **Semantic landmarks**: `<header>`, `<aside class="col left|right">`,
  `<main class="center">`. Use them; don't replace with `<div>`.
- **IDs are camelCase** and descriptive: `txtInput`, `expFront`, `mBorder`,
  `gHoleD`, `modelName`. Grab them in JS with `$('#id')` (see §5).
- **Classes are lowercase, flat, short**: `.col`, `.sec`, `.row`, `.btnrow`,
  `.note`, `.chk`, `.hint`, `.lib`, `.empty`. Modifier = second class, not
  BEM underscores: `.exportBtn.ghost`, `.layerItem.sel`, `.sidetabs button.active`.
- **Icon-only / ambiguous controls need labels**: `title=""` on inputs,
  `aria-label=""` on icon buttons, `role="tablist"`/`role="tab"` on the tab
  strip, `aria-hidden="true"` on decorative `.logo`.
- **Section fences as HTML comments**: `<!-- LEFT : library -->`,
  `<!-- CENTER -->`, `<!-- RIGHT -->`.
- **Hidden file inputs** (`<input type="file" … hidden>`) paired with a visible
  `<button>` — keep this pattern for new uploads.

---

## 4. CSS conventions

- **Design tokens live in `:root`** and nowhere else. Reuse them via
  `var(--name)`. Do not hardcode hex where a token exists.

  | Token | Use |
  |-------|-----|
  | `--bg`, `--panel`, `--panel2` | page + panel backgrounds |
  | `--line` | borders/dividers |
  | `--text`, `--dim` | primary vs secondary text |
  | `--gold`, `--gold-dk` | brand accent (active states, CTAs) |
  | `--silver` | neutral vector previews |
  | `--danger`, `--ok` | destructive / success |
  | `--mono`, `--disp` | font stacks (IBM Plex Mono / Space Grotesk) |

- **One theme — dark.** Do not add a light theme or theme-switching.
- **Units are `px`.** This is a physical-tool UI; `px` is deliberate. Don't
  switch to `rem`/`em` for layout. The only `mm` in the file is the dims
  readout text, not CSS units.
- **Compact one-liners are welcome** where readable:
  `*{box-sizing:border-box;margin:0;padding:0;…}`. Match density to context.
- **Responsive**: one breakpoint, `@media (max-width:980px)`, which collapses
  the 3-column grid into a stacked flex column. Keep new layouts working at
  ≥980px (desktop) and under 980px (mobile stack).

---

## 5. JavaScript conventions

- Start the `<script>` with `'use strict';` (already present).
- **`const $ = s => document.querySelector(s);`** is the only DOM accessor
  pattern. Use `$('#id')` / `$('.class')`. Do not introduce
  `getElementById`, jQuery, or a framework.
- **Single quotes** for all strings. No double quotes except in HTML
  attributes/JSX-like contexts (there is no JSX here).
- **`const` by default**, `let` only when reassignment is required. No `var`.
- **Arrow functions** are the default; use them for helpers and callbacks.
  Named `function` declarations are fine for top-level sections (the vector
  library uses them).
- **Data as object literals**, UPPER-CASED const names:
  - `MODELS` — immutable YubiKey catalogue (mm dimensions, connector/hole/pad, style).
  - `KEY` — **live, calibratable** copy of the active model (the calibrate panel
    mutates this, never `MODELS`). This split is load-bearing — respect it.
  - `LIB` — vector motif registry.
  - `GOLD` / `MASK` — shared style + mask-clearance defaults.
  - `state` — the only mutable runtime object (`side`, `layers{front,back}`,
    `sel`).
- **camelCase** for all identifiers: `addLayer`, `applyModel`, `maskPath`,
  `defaultY`, `roundedRect`, `clamp`. No snake_case, no PascalCase for funcs.
- **Path2D for all vector geometry.** Motifs are built in a **-1..1 unit box**
  and filled with the `'evenodd'` rule. New motifs must follow this contract so
  the shared transform/scale code works.
- **Guard risky DOM/shape code** with `try/catch` (see the library-build loop:
  a failing shape logs and is skipped, never crashes the app). Match this for
  any new procedural geometry.
- **No modules / no imports.** Everything shares one module scope. Order
  matters: helpers and data catalogues are declared before the code that uses
  them.

---

## 6. Worked examples — adding things the right way

### Add a YubiKey model
Add an entry to the `MODELS` object literal (don't mutate `KEY` directly —
`applyModel(id)` clones the chosen model into `KEY`). All values in mm. Origin
is top-left of the `W × H` bounding box, connector at the top, keyring hole at
the bottom.

```js
'new-id': {label:'YubiKey …', W:18, H:45, bodyTop:6.2, bodyR:3.0,
           conn:{type:'usbc',w:9.0,h:6.6,r:1.6}, hole:{d:4.0,y:40.8},
           pad:{d:11.4,y:25.6}, style:GOLD, nfc:true},
```

### Add a vector motif
1. Write a `function motifName(){ const p=new Path2D(); …; return p; }` in the
   VECTOR LIBRARY banner, geometry in the **-1..1 unit box**.
2. Register it in `LIB`:
   `{name:'Motif', make:motifName},`
3. It auto-appears in the library grid and the layer-add flow — no other wiring.

### Add a UI control
Give it a camelCase `id`, place it under the right `.sec`, style with existing
tokens/classes, and wire it with `$('#id')`. If it changes geometry or export,
call the existing `refresh()` / re-render path rather than drawing ad-hoc.

---

## 7. Explicitly forbidden

- Splitting the file; adding any build step, bundler, transpiler, or `package.json`.
- Any external dependency besides Google Fonts.
- A framework (React/Vue/etc.), a state library, or a router.
- Changing export polarity away from **black = engrave**.
- Emitting an exported PNG without the `pHYs` DPI chunk.
- Hardcoding hex/px where a `--token` / mm unit already applies.
- `var`, `getElementById`, double-quoted JS strings, snake_case JS identifiers.
- Breaking the desktop (>980px) / mobile (≤980px) layouts.

---

## 8. Pre-commit checklist

- [ ] App still opens from `file://` — no console errors, no network 404s.
- [ ] Single file, no new external dependencies.
- [ ] Design tokens reused; no stray hardcoded colours/units.
- [ ] If you touched export/mask/scale: exported PNG still lands at true physical
      size in LightBurn, polarity black = engrave, clip mask still correct.
- [ ] Works at desktop width **and** ≤980px mobile stack.
- [ ] Header comment + this guide still accurate (update them if you added a
      feature or model).

---

*Author: Stephen Eaton. Update this guide when the file's conventions change —
it is the contract between the code and the next AI that touches it.*
