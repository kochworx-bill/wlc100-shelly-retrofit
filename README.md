# wlc100-shelly-retrofit

DIY guide to replacing a dead WindowMaster WLC 100 skylight controller with a
Shelly 2PM Gen4 + OONO relay module, reusing the original Velux/WindowMaster
KEM 140 motors and existing wiring.

📖 **[Read the full guide](https://kochworx-bill.github.io/wlc100-shelly-retrofit/)**

## What's in this repo

This repo is a Jekyll site published via GitHub Pages. The full write-up
covers:

- [Diagnosis](docs/diagnosis.md) — confirm your WLC 100/KEM 140 hardware and
  identify whether the power supply, the motor's internal PCB, or both have
  failed
- [Parts List](docs/parts-list.md) — bill of materials (Mean Well LRS-100-24,
  Shelly 2PM Gen4, OONO F-1020, RC snubbers, pushbuttons)
- [Architecture](docs/architecture.md) — system overview, signal path, and
  full wiring table
- [Build Guide](docs/build-guide.md) — step-by-step wiring and assembly
- [Shelly Configuration](docs/shelly-configuration.md) — app/web setup
  walkthrough, including Cover mode setup and calibration
- [Troubleshooting](docs/troubleshooting.md) — known pitfalls and fixes
- [Limitations](docs/limitations.md) — known limitations and planned
  improvements

## Why

WLC 100 controllers are discontinued and their internal power supply boards
commonly fail. The KEM 140 motors they drive also have an internal PCB that
can fail independently. Rather than replacing motors and rack-and-pinion
hardware, this project bypasses both dead components and drives the motors
directly with a Shelly 2PM Gen4 and an OONO forward/reverse relay module,
reusing the original wiring runs wherever possible.

## ⚠️ Disclaimer

This is hobbyist documentation, not professional electrical advice. It
involves mains-powered supplies and DC motor wiring. Always work with power
off and verify circuits are de-energized with a multimeter before touching
any wiring. See the [full disclaimer](index.md#️-safety-disclaimer) for
details.

## Local preview

```sh
bundle install
bundle exec jekyll serve
```

Then open `http://localhost:4000/wlc100-shelly-retrofit/` in a browser.
