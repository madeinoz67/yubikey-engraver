# YubiKey 5C NFC · Engraving Designer

**🌐 Live demo: <https://madeinoz67.github.io/yubikey-engraver/>**

A single-file, local-first web app for designing laser-engraving artwork for
**every YubiKey / security-key form factor — all models except the Nano**
(the flush USB-stick Nano has no exposed surface to engrave). Open the HTML
file in any modern browser — **no server, no install, no build step.**

![YubiKey 5C NFC Engraving Designer](screenshot.png)

Author: **Stephen Eaton** · License: **MIT**

## Features

- Dimensionally-accurate YubiKey outline; design **front and back** artwork
- Built-in **vector motif** library + **JPEG/PNG upload** + **text layers**
- Move (drag / arrow keys), scale (wheel / slider), rotate (Alt+wheel / slider)
- **Front mask clips:** adjustable border, keyring hole, touch pad
- **Back mask clips:** adjustable border, keyring hole
- Live **engraved preview** (light-on-black) for both sides
- **Export** clipped artwork as PNG at true physical scale (`pHYs` DPI embedded),
  ready for LightBurn / laser-engraving software (**black = engrave**)

## Supported models

Built-in presets for the common engraveable YubiKeys:

- YubiKey 5C NFC · YubiKey 5C (compact)
- YubiKey 5 NFC (USB-A)
- YubiKey Bio C (fingerprint)
- Security Key C NFC

Any other engraveable key can be dialled in with the **calibrate** panel
(measure W / H / hole / pad in mm with calipers). **Nano form-factor keys are
not supported** — the flush USB-stick body leaves no surface to engrave.

## Use

1. Open `yubikey-engraver.html` in Chrome / Firefox / Safari — or use the
   [hosted version](https://madeinoz67.github.io/yubikey-engraver/).
2. Pick your model (or calibrate a custom one), then design — drop in a motif
   or image, add text, position with drag / arrows / wheel.
3. **Export PNG** → import into LightBurn (or your laser software) → engrave
   (black = engrave).

## Local development

It's a single static HTML file. Edit, refresh the browser, done. No
dependencies, no build chain.

## Developing with AI

See [`docs/STYLEGUIDE.md`](docs/STYLEGUIDE.md) — the conventions, invariants,
and forbidden changes any AI (or human) contributor must follow when editing
the app.

## Deploy (GitHub Pages)

Pushing to `main` triggers [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml),
which publishes the app to GitHub Pages. The workflow stages
`yubikey-engraver.html` as `index.html` so the site root serves the app
directly at <https://madeinoz67.github.io/yubikey-engraver/>.

## License

MIT — see [LICENSE](LICENSE).
© 2026 Stephen Eaton
