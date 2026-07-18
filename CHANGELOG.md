# Changelog

All notable changes to this project are documented here.
Format based on [Keep a Changelog](https://keepachangelog.com/),
following [Semantic Versioning](https://semver.org/).

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
