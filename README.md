# Vax! — Galvoro Capsule Fork

This repository is a Galvoro Capsule fork of [cadms/VaxGame](https://github.com/cadms/VaxGame), itself a static refactor of the original Ruby-on-Rails-based Vax! by the [Digital Epidemiology Lab / Salathé Group](https://github.com/digitalepidemiologylab/VaxGame). It is published as a single, self-contained capsule on Galvoro at `<slug>.capsules.galvoro.app`.

Vax! is a game about epidemic prevention. Players prepare for an outbreak by vaccinating a network that resembles human social networks; after distributing vaccines, an infectious outbreak begins to spread and the player must quell the epidemic by quarantining individuals at risk of becoming infected.

## Attribution chain

1. **Original** — Ellsworth Campbell, PhD student in the Salathé Group at Penn State University. Graphic design by Isaac Bromley. Released by the Digital Epidemiology Lab at EPFL: <https://github.com/digitalepidemiologylab/VaxGame>. Originally a Ruby-on-Rails web application.
2. **Static refactor** — `cadms/VaxGame` <https://github.com/cadms/VaxGame> stripped out the Rails server and turned Vax! into a standalone HTML/CSS/JS app. This is the direct upstream of the present fork.
3. **This Galvoro fork** — packages the static app as a Galvoro Capsule per the [Capsule Contribution Spec](capsule-spec.md). Gameplay, simulation logic, and visual design are unchanged from upstream.

## License

CC-BY-SA-4.0 (see `LICENSE`). The license is inherited from the original Salathé Group release and propagated through the cadms refactor; this fork does not change it. ShareAlike applies — derivative works must use the same license.

## Deviations from cadms/VaxGame upstream

These changes were applied solely to satisfy Galvoro's Capsule Contribution Spec. None of them affect gameplay.

- **Removed the standalone Herd Immunity module** (`herdImmunity.html`, `stylesheets/herdImmunity.css`, and the supporting `javascripts/herdImmunity.js`, `hiBarChart.js`, `hiNET.js`, `hiScript.js`, `hiSims.js`). A capsule teaches one concept; the herd-immunity exploration is conceptually a separate capsule.
- **Removed all Rails artifacts** that survived the cadms refactor: `400.html`, `404.html`, `406-unsupported-browser.html`, `422.html`, `500.html`, and the Rails-style `.gitignore` content (replaced with a minimal dev-time-only one).
- **Removed the social-share buttons** (Facebook, Twitter, Google+) from the Game and Scores screens. Outbound top-level navigation is blocked in the capsule sandbox iframe, the `vax.herokuapp.com` host they referenced no longer exists, and Google+ has been retired. The associated `images/{facebook,twitter,googleplus}_icon.png` files are also removed.
- **Converted external hyperlinks to plain text** in the FAQ. URLs that genuinely help readers (the Coursera Epidemics course, the CDC quarantine page) are kept as plain-text URLs; brand/affiliation links (Twitter, LinkedIn, salathegroup.com, psu.edu, rubyonrails.org, d3js.org, etc.) are dropped entirely. The cadms upstream GitHub link is preserved through the attribution chain in this README and in `capsule.json`.
- **Replaced absolute URL paths** (`window.location.href = '/'`, `'/game.html'`, `'/scenario'`, etc.) with relative file references (`'index.html'`, `'game.html'`, `'scenario.html'`). Galvoro serves capsules from versioned subpaths (`<slug>.capsules.galvoro.app/v1/`); absolute paths break versioned URL routing.
- **Stripped commented-out analytics infrastructure** from `faq.html` (Google Analytics blurb and an `iubenda` privacy-policy script). The code was already disabled in cadms but the strings still shipped and would have tripped Galvoro's static-check scanner.
- **Added `capsule.json`** at the repo root with the metadata Galvoro requires (title, learning goals, level, fields, license, etc.).

## Known issues / not yet addressed

These are flagged for a follow-up cleanup pass; they do not affect the gameplay this fork ships:

- `images/pop.wav` and `images/vax_cursor.cur` use file extensions outside the §4 capsule allowlist (only `.mp3` / `.ogg` for audio, no `.cur`). They will need to be re-encoded or removed before Galvoro static-check passes.
- `tutorial.js` references an audio element ID that depends on the `.wav` above — if the audio file is removed, the reference should be cleaned up too.

## Running locally

The capsule has no build step. From the repo root:

```
python3 -m http.server 8000
```

Then open <http://localhost:8000/>. Any plain static file server works equivalently.

## Capsule spec

The spec this fork targets is in `capsule-spec.md` at the repo root. When that document and this README disagree, the spec wins.
