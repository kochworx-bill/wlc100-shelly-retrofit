---
layout: home
title: Home
nav_order: 1
---

# Reviving a Dead Skylight Controller: WLC 100 → Shelly Retrofit

If your Velux/WindowMaster skylights suddenly stopped responding to their wall
keypads, and you've traced it back to a dead **WLC 100** controller, you're in
the right place. This site documents a complete, working retrofit that:

- Replaces the discontinued WLC 100 controller and its failure-prone power
  supply board
- **Reuses your existing KEM 140 rack-and-pinion motors** — no need to replace
  the motors or re-run wiring through your ceiling
- **Reuses your existing wall keypad wiring** as plain signal wires for new
  momentary pushbuttons
- Adds WiFi/Zigbee/Matter smart-home control via a Shelly 2PM Gen4, so you can
  open and close your skylights from an app, automation, or physical button

## Why this exists

WindowMaster WLC 100 controllers are no longer manufactured, and replacement
units are hard to find. The internal power supply board is a common failure
point, and the KEM 140 motors themselves have an internal PCB (with limit
switches) that can fail independently. When either part dies, the entire
skylight becomes inoperable — even though the motor itself is often still
perfectly healthy.

Rather than replacing the motors and rack-and-pinion hardware (expensive and
invasive), this project bypasses the dead controller and motor PCB, and
drives the motor directly with modern, inexpensive, off-the-shelf components.

## The author's journey

The author's four skylights worked fine for about a decade before one pair
quietly stopped responding to its wall keypad. At the time, unrelated wall
work nearby made a damaged wire seem like the obvious culprit, and the
non-opening windows were shelved as a problem for another day — hand-cranking
a skylight 16 feet up a cathedral ceiling isn't exactly a weekend chore.

A few years later, with more time at home and more reason to want working
skylights again, the author went looking for answers and found what a lot of
WLC 100 owners eventually find: the controller is discontinued, its power
supply board is a known failure point, and there wasn't a clear off-the-shelf
replacement. That search stalled for a while, until modern AI coding
assistants made it practical to work through diagnosis, parts selection, and
wiring step by step, rather than needing to already be a WindowMaster
specialist.

This guide is the result of that process — written so a reader facing the
same dead controller doesn't have to start from zero the way the author did.

## How to use this guide

Work through the pages in order:

1. **[Diagnosis](docs/diagnosis.md)** — Confirm this is actually your problem
   before buying parts.
2. **[Parts List](docs/parts-list.md)** — What to buy, and why.
3. **[Architecture](docs/architecture.md)** — How signal and power flow from
   button/app to motor.
4. **[Build Guide](docs/build-guide.md)** — Step-by-step wiring and assembly.
5. **[Shelly Configuration](docs/shelly-configuration.md)** — App/web setup
   walkthrough, including the one confusing UI step almost everyone misses.
6. **[Troubleshooting](docs/troubleshooting.md)** — Pitfalls discovered the
   hard way, so you don't have to repeat them.
7. **[Limitations](docs/limitations.md)** — What this solution doesn't do
   (yet), and planned improvements.

## ⚠️ Safety Disclaimer

This is a hobbyist DIY project write-up, **not professional electrical or
engineering advice**. It involves working with low-voltage DC motor wiring
and a mains-powered switching supply. Before you touch any wiring:

- **Turn off power** at the breaker or unplug the supply before opening any
  enclosure or making/breaking connections.
- **Verify circuits are de-energized with a multimeter** before touching bare
  conductors — never assume a switch or breaker label is correct.
- If you are not comfortable working with electrical wiring, hire a licensed
  electrician. Skylights are often on dedicated circuits at height, adding
  fall-hazard risk on top of electrical risk.
- Follow your local electrical code. This project is documented as-built for
  one installation and may not be appropriate for yours.

Proceed at your own risk. The author(s) of this guide are not liable for
damage, injury, or code violations resulting from following it.
