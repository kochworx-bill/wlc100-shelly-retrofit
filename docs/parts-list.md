---
layout: page
title: Parts List
permalink: /docs/parts-list/
nav_order: 3
---

# Bill of Materials

This is a per-window retrofit, except for the power supply and distribution
block, which are shared across all windows fed from one location. Pricing
and quantities below are from a real **2-skylight** install — scale wire
length and per-window items up or down for your own job.

| Item | Price | Qty for 2 Skylights | Link |
|---|---|---|---|
| Mean Well LRS-100-24 (24V DC, 4.5A, 108W switching power supply) | $18.99 | 1 shared | [Amazon](https://a.co/d/01V15kK7) |
| 30A 48V 2x6 Position Terminal Block Distribution Module | $12.99 | 1 shared | [Amazon](https://a.co/d/09Yl3Fa8) |
| Shelly 2PM Gen4 Smart Relay Switch (WiFi/Matter/Zigbee, 2-channel), 2-pack | $75.99/2 pcs. | 1 pack (2 units — one per window) | [Amazon](https://a.co/d/0fZlRA1G) |
| OONO Forward/Reverse Relay Module for Motor/Linear Actuator (DC 24V) | $17.40 each | 2 (one per window) | [Amazon](https://a.co/d/0affWjFu) |
| KCD3-123 Momentary Rocker Switch, SPDT (ON)-Off-(ON), 3-pin, 6-pack | $9.99/6 pcs. | 1 pack | [Amazon](https://a.co/d/08OgQQFg) |
| 18/3 Solid Core Wire | $0.50/ft. | Depends on installation (assume 30 ft. ≈ $15.00) | [Amazon](https://a.co/d/07y2zjOB) |
| RC Absorption Snubber Circuit Module (relay contact protection), 10-pack | $9.99/10 pcs. | Optional / 0 | [Amazon](https://a.co/d/0acOLpbk) |

## Notes on part selection

**Why the Mean Well LRS-100-24 specifically?** It's a well-regarded,
readily available industrial 24V supply with enough current capacity (4.5A)
to run several KEM 140 motors plus relay coil draw without being oversized
for a residential closet install.

**Why a terminal block distribution module?** The Mean Well supply only has
one set of output terminals, but you need to feed 24V DC out to multiple
Shelly units, OONO modules, and (if used) rocker switches. A 30A-rated
terminal block lets you split the single 24V DC output into as many home
runs as you need without stacking multiple wires under one screw terminal,
which is unreliable and can loosen over time.

**Why 18/3 solid core wire?** Three conductors per run covers the +V, −V,
and a signal/return conductor needed for each drop (e.g., to wall switches
or between the distribution block and a Shelly/OONO pair). Actual footage
depends heavily on how far your enclosures are from the shared power supply
and from each other — measure your own runs before ordering.

**What are the KCD3-123 rocker switches for?** These are an alternative (or
supplement) to doorbell-style momentary pushbuttons for manual wall
control — a 3-pin (ON)-Off-(ON) SPDT rocker gives you a single switch that
momentarily makes contact in either direction (open or close) and returns
to center, rather than needing two separate buttons. Use whichever style
fits your wall plate and aesthetic; the wiring pattern in
[Architecture](architecture.md) applies either way.

**Why Shelly 2PM Gen4?** Two independent relay channels per unit, native
Cover/shade device mode (built for exactly this kind of open/close motor
control), and WiFi/Zigbee/Matter support means it fits into most existing
smart home ecosystems.

**Why not just wire the Shelly directly to the motor?** The Shelly 2PM's
relay outputs are simple on/off switches — they can't reverse DC polarity
on their own, and doing so with pure relay logic on the Shelly would require
external wiring anyway. The OONO F-1020 is a purpose-built forward/reverse
module that takes two logic-level trigger inputs and switches DC polarity
using its own internal relays, which is exactly what's needed to make a
DC gearmotor go one direction on "open" and the other on "close."

**Do I need the RC snubber?** It's listed as optional above — the system
functions without it, and the reference 2-skylight build didn't use one.
That said, relay contacts switching an inductive load (a motor) without
suppression wear out faster over time and can cause electrical noise that
affects nearby electronics. At roughly $1/unit, it's cheap insurance if you
want to add one across each motor's output leads.

Once your parts arrive, move on to [Architecture](architecture.md) to
understand how everything connects before you start wiring.
