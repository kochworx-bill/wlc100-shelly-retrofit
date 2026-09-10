---
layout: page
title: Architecture
permalink: /docs/architecture/
nav_order: 4
---

# System Overview and Signal Path

## The big picture

```
Mean Well LRS-100-24 (24V DC)
        |
        v
Shared terminal block distribution module
        |
        +-----------------------------+
        v                             v
Wall button  OR  Shelly app/automation
        |
        v
  Shelly 2PM Gen4  (S1/S2 inputs, O1/O2 outputs)
        |
        v
  OONO F-1020  (FWD/REV trigger inputs)
        |
        v
  KEM 140 motor  (wired directly to motor terminals,
                   bypassing the internal motor PCB)

  (optional) RC snubber across motor leads, spliced in
  at the terminal block before the run to the motor
```

*(Add a photo or hand-drawn diagram of this signal path to `images/` and
reference it here, e.g. `![Signal path diagram](../images/signal-path.png)`)*

Every window gets its own Shelly 2PM Gen4 and its own OONO F-1020 — see
[Troubleshooting](troubleshooting.md) for why sharing one Shelly/OONO pair
across two motors causes overcurrent trips. All windows share a single
Mean Well LRS-100-24 power supply and a single terminal block distribution
module, since the supply only has one set of output terminals but needs to
feed multiple Shelly/OONO pairs. The distribution block splits the 24V DC
output into as many home runs as needed without stacking multiple wires
under one screw terminal.

## Why the OONO module is necessary

The KEM 140 is a DC gearmotor: applying 24V DC in one polarity drives it to
open, and reversing polarity drives it to close. The Shelly 2PM Gen4's two
relay outputs are simple on/off switches — they don't reverse polarity on
their own. The OONO F-1020 sits between the Shelly and the motor: it accepts
two trigger inputs (FWD and REV) and internally switches which DC leads get
+V and which get 0V, producing the polarity reversal the motor needs.

## DC wiring polarity — read this before wiring your Shelly

The Shelly 2PM Gen4 in DC mode is wired **opposite** to how you'd expect from
AC household wiring conventions:

- **Mean Well −V → Shelly L** (the terminal marked with the ⊥ ground-like symbol)
- **Mean Well +V → Shelly N** (the terminal marked with the + symbol)

This trips people up because "L" and "N" read like AC line/neutral labels,
but in DC mode they're just terminal names — always check the physical label
printed on your specific Shelly unit before connecting power, since terminal
layouts can change between hardware revisions.

## Full wiring table (per window)

| From | To |
|---|---|
| Mean Well −V | Shelly L |
| Mean Well +V | Shelly N |
| Shelly O1 | OONO FWD |
| Shelly O2 | OONO REV |
| Mean Well +V | OONO V+ |
| Mean Well −V | OONO 0V (top and bottom) |
| OONO M1 | KEM 140 motor wire 1 |
| OONO M2 | KEM 140 motor wire 2 |
| *(optional)* RC Snubber | across OONO M1/M2, spliced in at the terminal block before the run to the motor |
| Wall button 1 (open) | Shelly S1 + Mean Well −V |
| Wall button 2 (close) | Shelly S2 + Mean Well −V |

A few things worth calling out from this table:

- The **OONO's two 0V terminals** (top and bottom) both need to connect back
  to the Mean Well's −V — don't assume one is unused.
- The **RC snubber is optional** and was not used in the reference build,
  favoring simplicity. If added later, it's spliced in across the OONO's
  M1/M2 output terminals at the shared terminal block, ahead of the run out
  to the motor — not anywhere else in the circuit. This keeps it easy to
  retrofit per window without redoing the motor wiring itself.
- **Wall buttons are wired to the Shelly's input terminals and common
  return (Mean Well −V)** — they're simple momentary contacts, no power runs
  through the button itself. See [Build Guide](build-guide.md) for how this
  maps onto reused WLC wall-keypad wiring.

## What got bypassed, and why

Two things in the original system are intentionally bypassed:

1. **The WLC 100 controller itself** — including its internal power supply
   board (replaced by the Mean Well LRS-100-24) and its WindowMaster-specific
   digital keypad bus protocol (replaced by plain momentary switch wiring).
2. **The KEM 140 motor's internal PCB** — including its limit switches. The
   new wiring connects directly to the motor terminals, not the harness
   wires that used to pass through this board. This is why end-of-travel
   detection is now handled differently — see [Limitations](limitations.md)
   for details on how this is currently addressed and what's planned.

With the wiring plan understood, move on to the [Build Guide](build-guide.md)
for step-by-step assembly instructions.
