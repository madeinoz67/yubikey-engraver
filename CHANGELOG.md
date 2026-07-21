# Changelog

All notable changes to this project are documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/),
following [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [0.4.0] — 2026-07-21

Per-layer mirror + Hackaday logo motif.

### Added
- **Per-layer Mirror (H/V)** — flip the selected layer horizontally or
  vertically (Mirror H / Mirror V buttons in the properties panel; `H` / `V`
  keyboard shortcuts). Mirroring both == a 180° rotation. Works for motifs,
  text, and images; save/load round-trips it. The existing export-time
  `expMirror` (global back-side flip) is unchanged.
- **Hackaday Jolly Wrencher motif** — the Hackaday mascot (skull + crossed
  wrenches) added as the first entry in the vector motif library, from the
  official logo path data (3 separate paths, normalised to the unit box).
- **Version badge** in the top header + a **copyright footer** bar.

### Changed
- Default text-entry placeholder genericised to `e.g. MY KEY`.

## [0.3.0] — 2026-07-20

Black-YubiKey engraving polarity.

### Changed
- **Image layers now default to Invert on** (light art engraves). On a black
  YubiKey, laser engraving reveals a lighter underlayer, so you want your
  (light) design engraved — leaving the dark background (the key body)
  untouched. Imported artwork is typically light-on-dark, so defaulting
  invert on makes the design engrave out of the box instead of the
  background. Uncheck for dark-on-light artwork.

### Docs
- README: new **Engraving polarity (dark vs light)** section — explains that
  `black = engrave` becomes a light mark on a black key, when to use Invert,
  and that the engraved preview and the export PNG are two views of the same
  engraving.

## [0.2.1] — 2026-07-19

Colour sticker printing (Cricut print-and-cut / UV).

### Added
- **STICKER/ENGRAVE view toggle** (header) — drives the editor canvas and the
  front/back preview thumbnails. ENGRAVE = the existing monochrome laser view;
  STICKER = full-colour (per-layer colours + image RGB).
- **Per-layer colour** for vector and text layers — colour picker in the
  properties panel; the chosen colour shows in STICKER view and the sticker
  export.
- **Colour-retaining image layers** — uploaded images render as their natural
  image (original colours + their own transparency) in STICKER view and the
  sticker export. The laser path still uses the monochrome engrave-intensity
  bake.
- **Full-colour sticker export** (PNG + SVG) — dark body (`stBg`) + per-layer
  colours + image RGB, for Cricut print-and-cut and UV printing.

### Changed
- `drawLayerInto` / `renderArt` are now mode-aware (`'engrave'` default is
  identical to prior behavior; `'color'` drives sticker export + STICKER view).
- New motif/text layers default to the **gold accent** (was grey) so STICKER
  mode is visibly distinct from the monochrome engrave view; `stArt` is the
  default-art-colour swatch.
- Sticker PNG, SVG, and the STICKER-mode preview are cropped to the printable
  key body (`KEY.H − KEY.bodyTop`), matching the laser export (no connector
  strip).
- The view toggle reads **STICKER** (was COLOR), matching the "STICKER PREVIEW"
  thumbnail captions.

### Fixed
- Colour picker no longer closes on the first drag — the properties panel is
  not rebuilt while picking, so you can browse and compare colours.
- Front/back preview captions now switch with the mode (STICKER / ENGRAVED).

## [0.1.0] — 2026-07-18

First release.

### Added
- Single-file engraving designer for YubiKey / security-key form factors —
  all models except the Nano (no engraveable surface). Front and back artwork
  over a dimensionally-accurate outline.
- Built-in vector motif library (star, bolt, heart, shield, hexagon, gear,
  reticle, atom, snowflake, padlock, key, radiation, yin-yang, crescent, paw,
  skull, Wi-Fi, circuit) + JPEG/PNG image upload + text layers.
- Move (drag / arrow keys), scale (wheel / slider), rotate (Alt+wheel / slider);
  per-layer transforms and ordering.
- Adjustable clip mask — border, keyring hole, touch pad (front only).
- Live engraved preview (light-on-black) for both sides.
- Calibrate panel to dial in any key's real geometry (mm) with calipers.
- PNG export at true physical scale (`pHYs` DPI embedded), LightBurn-ready
  (**black = engrave**); back-side mirror option; body-outline alignment toggle.
- Sticker export (SVG + PNG) for Cricut print-and-cut.
- Save / load project as JSON.
- GitHub Pages auto-deploy on push to `main`.

### Fixed
- PNG export no longer includes a blank connector strip at the top — output is
  tightly cropped to the engraveable key body (`KEY.H − KEY.bodyTop`).
