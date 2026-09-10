---
title: Wiring Diagram & Build Guide
---

# Wiring Diagram & Connection Table

This page documents the exact wiring used to replace a single dead WindowMaster
WLC 100 controller circuit with a Shelly 2PM Gen4 + OONO F-1020 relay module,
reusing the original KEM 140 motor with its internal limit-switch PCB bypassed.

## Schematic

![Skylight retrofit wiring schematic](images/wiring-schematic.jpg)

*Diagram reflects the author's actual physical board layout and component
orientation. Text on some components (Mean Well, OONO, distribution block)
may be hard to read at full resolution — always cross-reference against the
physical unit's printed terminal labels before wiring. See the
[Parts List](parts-list.md) page for direct photos of each labeled component.*

**Key callouts in the diagram:**

- **Disconnect motor from board and connect to M1/M2** — the KEM 140's
  internal circuit board (which originally handled limit switches) is bypassed
  entirely. The two motor power leads are wired directly from the OONO
  module's M1/M2 terminals straight to the motor's power terminals.
- **Rocker switch center connector goes to the Mean Well V- (B leg of distro
  block)** — the reused 3-wire wall switch uses its center/common wire as the
  shared return path to the power supply's negative rail.
- **Reuse 3 wires from old wall controller, add additional wires for more
  skylight motors** — the original WindowMaster WLL bus wiring (3 conductors)
  is repurposed as plain signal wire once the proprietary digital protocol is
  abandoned: one wire per direction (open/close), one shared common return.

---

## Wiring Summary Table

| From | To |
|---|---|
| Mean Well +V | Distribution block + rail |
| Mean Well -V | Distribution block - rail |
| Distribution block + rail | Shelly N (+) |
| Distribution block - rail | Shelly L (⊥) |
| Distribution block + rail | OONO V+ |
| Distribution block - rail | OONO 0V (top, control ground) |
| Distribution block - rail | OONO 0V (bottom, power ground) |
| Shelly O1 | OONO FWD |
| Shelly O2 | OONO REV |
| Shelly S1 | Rocker switch (Open side) |
| Shelly S2 | Rocker switch (Close side) |
| Rocker switch (center/common) | Distribution block - rail (Mean Well -V) |
| OONO M1 | KEM 140 motor lead 1 (direct to motor, bypassing internal PCB) |
| OONO M2 | KEM 140 motor lead 2 (direct to motor, bypassing internal PCB) |
| RC Snubber | Across OONO M1 and M2 at motor terminals |

> **Note on Shelly DC wiring polarity:** The Shelly 2PM Gen4's L terminal is
> marked with a ground/negative symbol (⊥) and the N terminal with a positive
> symbol (+) for DC operation — this is reversed from typical AC wiring
> convention. Always verify against the physical label printed on your own
> unit before connecting power.

---

## Safety Reminder

This project involves both line-voltage AC wiring (Mean Well power supply
input) and low-voltage DC motor wiring. Always disconnect power at the
breaker before working on any connections, and verify wiring with a
multimeter before applying power. This guide is not a substitute for
professional electrical advice — proceed at your own risk.
