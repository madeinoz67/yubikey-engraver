# YubiKey 5C NFC · Engraving Designer

A single-file, local-first web app for designing laser-engraving artwork for
YubiKey 5C NFC keys (and other YubiKey / security-key form factors). Open the
HTML file in any modern browser — **no server, no install, no build step.**

Author: **Stephen Eaton** · License: **MIT**

## Features

- Dimensionally-accurate YubiKey 5C NFC outline; design **front and back** artwork
- Built-in **vector motif** library + **JPEG/PNG upload** + **text layers**
- Move (drag / arrow keys), scale (wheel / slider), rotate (Alt+wheel / slider)
- **Front mask clips:** adjustable border, keyring hole, touch pad
- **Back mask clips:** adjustable border, keyring hole
- Live **engraved preview** (light-on-black) for both sides
- **Export** clipped artwork as PNG at true physical scale (`pHYs` DPI embedded),
  ready for LightBurn / laser-engraving software (**black = engrave**)

## Use

1. Open `yubikey-engraver.html` in Chrome / Firefox / Safari — or use the
   [hosted version](https://madeinoz67.github.io/yubikey-engraver/).
2. Design — drop in a motif or image, add text, position with drag / arrows / wheel.
3. **Export PNG** → import into LightBurn (or your laser software) → engrave
   (black = engrave).

## Local development

It's a single static HTML file. Edit, refresh the browser, done. No
dependencies, no build chain.

## Deploy (GitHub Pages)

Pushing to `main` triggers [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml),
which publishes the app to GitHub Pages. The workflow stages
`yubikey-engraver.html` as `index.html` so the site root serves the app
directly at <https://madeinoz67.github.io/yubikey-engraver/>.

## License

MIT — see [LICENSE](LICENSE).
© 2026 Stephen Eaton
