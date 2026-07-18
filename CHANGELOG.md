# Changelog

All notable changes to this project are documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/),
following [Semantic Versioning](https://semver.org/).

## [Unreleased]

Color support for sticker printing.

### Added
- **COLOR/ENGRAVE view toggle** (header) — drives the editor canvas and the
  front/back preview thumbnails. ENGRAVE = the existing monochrome laser view;
  COLOR = full-colour (per-layer colours + image RGB).
- **Per-layer colour** for vector and text layers — colour picker in the
  properties panel; the chosen colour shows in COLOR view and the sticker export.
- **Colour-retaining image layers** — uploaded images render as their natural
  image (original colours + their own transparency) in COLOR view and sticker
  export. The laser path still uses the monochrome engrave-intensity bake.
- **Sticker export is full-colour** — dark body (`stBg`) + per-layer colours +
  image RGB, for Cricut print-and-cut. `stArt` is now the default art colour
  for newly-added layers (gold accent, so COLOR mode is visibly distinct from
  the monochrome engrave view).

### Changed
- `drawLayerInto` / `renderArt` are now mode-aware (`'engrave'` default is
  identical to prior behavior; `'color'` drives sticker export + COLOR view).

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
