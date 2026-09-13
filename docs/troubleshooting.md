---
layout: page
title: Troubleshooting
permalink: /docs/troubleshooting/
nav_order: 7
icon: /docs/images/troubleshooting-icon.jpg
---

# Troubleshooting: Known Pitfalls and Fixes

Lessons learned the hard way, documented so you don't have to repeat them.

## 1. Reusing the old wall-keypad bus as plain signal wires

The original WLC wall-keypad wiring (often yellow/blue/red in older
installs) carried a WindowMaster-specific digital protocol between the WLC
100 and its WLI 130 keypads. Once you abandon that protocol entirely, these
are just three copper conductors — nothing about them is "smart."

**What worked:** one wire repurposed as a shared common return, one as the
open signal, and one as the close signal, feeding simple momentary
pushbuttons into the Shelly's S1/S2 inputs and Mean Well −V. See
[Build Guide, Step 5](build-guide.md#step-5-wire-wall-buttons-using-the-reused-keypad-wiring)
for the full wiring pattern.

## 2. Overcurrent trip from wiring two motors to one Shelly output

**Symptom:** connecting two KEM 140 motors to a single Shelly output (in an
attempt to save a Shelly/OONO pair by driving a matched pair of skylights
together) causes the channel to trip on overcurrent almost immediately.

**Cause:** both motors draw their startup surge current simultaneously when
switched on together, and the combined surge exceeds the Shelly channel's
10A rating — even though each motor individually draws well under that
limit at steady state.

**Fix:** wire **one Shelly and one OONO F-1020 per motor**, not per pair of
windows. This is reflected in the [Parts List](parts-list.md) — the
per-window quantities aren't just for independent control, they're required
to avoid this overcurrent condition.

**This fix is necessary but not sufficient on its own.** Even with a
dedicated Shelly and OONO per motor, the author has observed that running
two windows' motors at the same time — even when the commands aren't sent
at the exact same instant — can still trip an overcurrent condition and
stop both windows mid-travel. This points to the **shared Mean Well
LRS-100-24 power supply** running out of headroom when two motors draw
current simultaneously, rather than a per-channel Shelly/OONO problem. For
reliable operation, the author's practice is to operate only one window at
a time — this is a workflow habit, not an additional wiring fix, and it's
the current recommendation until a higher-capacity shared supply is tested.

## 3. Don't trust wire color at the WLC 100 terminal block

Installer wire colors frequently don't match the diagram molded into the
WLC 100's case label — someone doing the original install may have used
whatever wire was on hand, or a different color convention than the
manufacturer's diagram assumes.

**What works instead:** continuity testing.

1. At the skylight end, short the two motor leads together (power off).
2. At the WLC 100 terminal block, use a multimeter in continuity mode to
   probe pairs of terminals until you find the pair that shows continuity.
3. Label that pair as your confirmed motor leads for that window — don't
   rely on the wire's color matching the case label's diagram.

Repeat for each motor connector (M1/M2/M3) if you have multiple windows
feeding into the same WLC 100.

## 4. Use the WLC 100's molded case diagram as your wiring reference

Even though colors can't be trusted, the WLC 100 case itself has a wiring
diagram **molded directly into the plastic**, showing:

- `6` — rain sensor input
- `EXT` — external keypad bus
- `INT` — internal keypad bus (usually unused in residential installs)
- `M1` / `M2` / `M3` — motor connectors
- `24V` — power input

This is useful as a reference for understanding what each terminal on your
specific unit was originally used for, even after you've removed the WLC
100 from service. Photograph this diagram before you discard the old
controller — it's genuinely useful during the identification steps in
[Diagnosis](diagnosis.md) and [Build Guide](build-guide.md).

---

Have another pitfall to add? This list reflects one real-world install —
your wiring or symptoms may differ slightly. See
[Limitations](limitations.md) for known gaps in the current design.
