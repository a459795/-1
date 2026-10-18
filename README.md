# Claude Code Background Lab

A quiet field of monospace characters, carried by drifting wind and disturbed by your pointer. A small control panel lets you explore the animation's colors, typography, motion and timing.

**This project was REA'd — reverse-engineered with [Reverse Engineer Anything (REA)](https://github.com/morluto/rea) — from the Claude Code “New session” background in the Claude desktop app.** The investigation traced the desktop shell into the remote web bundle responsible for that view.

This public edition is an **original implementation informed by that study**, with an MIT-licensed renderer and control panel. It does not include Anthropic's recovered JavaScript, shaders, bundled font, app assets or private cache data. It uses system monospace fonts. It is an unofficial project, unaffiliated with Anthropic, and is not a pixel-identical extraction.

![Background and controls](docs/preview.jpg)

## Try it

Download or clone the repository and open **`index.html` in Chrome, Edge or another WebGL2 browser**. The page is a single offline file: no installation, account, API key, external fonts, server or network requests are needed.

```sh
git clone https://github.com/Henry-fan88/claude-code-background.git
cd claude-code-background
# Open index.html in your browser.
```

If your browser restricts local files, serve the directory:

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

Then visit `http://127.0.0.1:8000`.

## Play with it

- Move the pointer across the field to leave a wake. Click empty space for a blue ripple; every third click forms a heart.
- Adjust **38 sliders** covering appearance, motion, wind shape, pointer effects, the quiet center and timing.
- Choose colors, change the character set, edit the random seed or generate a new pattern.
- Pause the animation to inspect a still frame. Replay its entrance or reset the recovered parameter defaults.
- Save settings as JSON and load them later. Nothing is uploaded.
- Hide the panel with its minus button; use **Adjust animation** to reopen it.

Most controls update immediately. Grid size, font size/weight, character set and seed regenerate the field. Use Replay entrance to see timing changes. Fade-out controls the transition when the window loses focus. Reduced-motion preferences are respected. Font size is constrained to fit the cells, and intensity retains the original study's ceiling.

You can start with a repeatable pattern using `index.html?seed=123&look=mixed`. Supported looks are `mixed`, `lettered` and `figured`; their character sequences are original to this edition.

## What the study found

The effect combines a timed entrance, a continuously evolving glyph field and pointer interaction. It is not a fixed 7.5-second loop or a set of falling character streams.

The public implementation uses recovered design values such as 6.5 × 13 CSS-pixel cells, a nominal 9 px font, a 3 s entrance delay, 3 s fade-in, 4 s wind reveal, 500 ms focus fade-out, and seeded wind speeds of 42–64 px/s. The noise, shaders, glyph selection and rendering architecture are newly written for this repository.

See [provenance and implementation differences](docs/PROVENANCE.md) for the evidence boundary. The exact remote build, seed, font and theme active in the reference recording were not established.

## Develop

Python 3 and Node.js 18+ are sufficient. There are no third-party runtime or build dependencies.

```sh
python3 scripts/build.py
node --test scripts/renderer.test.cjs
python3 scripts/check_release.py
```

Edit files in `src/`, then rebuild. Commit both the source and generated `index.html`.

| File | Purpose |
| --- | --- |
| `src/renderer.js` | Original WebGL2 renderer, glyph atlas, wind, wakes and ripples |
| `src/controls.js` | Control panel and settings save/load |
| `src/adapter.js` | Panel-to-renderer integration |
| `src/styles.css` | Page and panel styling |
| `src/template.html` | HTML template |
| `scripts/build.py` | Embeds all source into the offline page |

GitHub Actions checks deterministic helpers, JavaScript syntax, release contents and whether the generated page is current. Browser validation is separate from those static checks; see [validation notes](docs/VALIDATION.md).

## License and credits

[MIT](LICENSE) for the original code and documentation in this repository. [Third-party notices](THIRD_PARTY_NOTICES.md) describe the reference and font boundary.

Visual reference: Claude Code's New session background in the Claude desktop app, by Anthropic. Reverse-engineering investigation: REA. Public reconstruction and controls: Henry Fan, with AI-assisted development.
