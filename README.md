# wlc100-shelly-retrofit

A self-documented, personal account of how the author replaced a dead
WindowMaster WLC 100 skylight controller with a Shelly 2PM Gen4 + OONO relay
module, reusing the original Velux/WindowMaster KEM 140 motors and existing
wiring. This is shared as an open-source reference for others who run into
the same discontinued-controller problem — it is **not** a professional
guide, a recommended standard, or the only way to solve this. See the full
disclaimer below and on the site's home page.

📖 **[Read the full write-up](https://kochworx-bill.github.io/wlc100-shelly-retrofit/)**

## What's in this repo

This repo is a Jekyll site published via GitHub Pages. It documents:

- [Diagnosis](docs/diagnosis.md) — confirm your WLC 100/KEM 140 hardware and
  identify whether the power supply, the motor's internal PCB, or both have
  failed
- [Parts List](docs/parts-list.md) — bill of materials (Mean Well LRS-100-24,
  Shelly 2PM Gen4, OONO F-1020, RC snubbers, pushbuttons)
- [Architecture](docs/architecture.md) — system overview, signal path, and
  full wiring table
- [Build Guide](docs/build-guide.md) — step-by-step wiring and assembly
- [Shelly Configuration](docs/shelly-configuration.md) — app/web setup
  walkthrough, including Cover mode setup and why Shelly's calibration
  routine cannot succeed on this build's relay-mediated wiring
- [Troubleshooting](docs/troubleshooting.md) — known pitfalls and fixes
- [Limitations](docs/limitations.md) — known limitations and planned
  improvements

## Why

WLC 100 controllers are discontinued and their internal power supply boards
commonly fail. The KEM 140 motors they drive also have an internal PCB that
can fail independently. Rather than replacing motors and rack-and-pinion
hardware, the author bypassed both dead components and drove the motors
directly with a Shelly 2PM Gen4 and an OONO forward/reverse relay module,
reusing the original wiring runs wherever possible. This documents that
one approach — other solutions may exist and may be better suited to a
different installation, skill set, or local code.

## ⚠️ Disclaimer

This site documents **one individual's personal, as-built approach** to a
problem they had with their own equipment, shared for reference only. It is
**not** professional electrical/engineering advice, not a recommended or
verified procedure, and not the only or safest way to solve this. It is
provided "as is," with no warranty of any kind. Anyone referencing, adapting,
or following anything described here does so entirely at their own risk,
and the author(s) disclaim all liability for any resulting damage, injury,
loss, or code violation. See the [full disclaimer](index.md#disclaimer) for
details, including required safety precautions before touching any wiring.

## Local preview

```sh
bundle install
bundle exec jekyll serve
```

Then open `http://localhost:4000/wlc100-shelly-retrofit/` in a browser.
