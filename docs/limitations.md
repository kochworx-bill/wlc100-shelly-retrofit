---
layout: page
title: Limitations
permalink: /docs/limitations/
nav_order: 8
---

# Known Limitations and Future Improvements

This retrofit works well in daily use, but it's worth being upfront about
what it doesn't do (yet), so you can decide whether it's the right approach
for your situation.

## No true end-of-travel limit switches

The KEM 140 motor's internal PCB originally provided limit-switch-based
end-of-travel detection — the motor would receive a hard electrical cutoff
signal when fully open or fully closed. Because this retrofit bypasses that
PCB entirely and wires directly to the motor terminals, that hard cutoff no
longer exists.

**Current mitigation:** the Shelly 2PM Gen4's Cover mode relies on:

- Calibrated travel time (measured once during setup, ~16–17 seconds)
- Built-in current-spike / obstacle detection, which senses the motor
  stalling against its mechanical end-stop and cuts power

This works reliably in practice, but it's a soft cutoff based on timing and
current sensing rather than a hard electrical/mechanical limit. It's
theoretically possible for calibration drift, mechanical binding, or a
firmware quirk to cause the motor to run slightly past where a true limit
switch would have stopped it, straining the rack-and-pinion mechanism.

**Planned improvement:** install external roller-lever micro limit
switches at each sash's fully-open and fully-closed positions, wired to
provide a hard cutoff independent of the Shelly's timing/current logic.
This is not yet implemented in this build.

## No rain sensor integration (yet)

The original WindowMaster system supported a rain sensor input (the `6`
terminal on the WLC 100) that could automatically close skylights when
precipitation was detected. This retrofit does not currently replicate that
functionality.

The planned path is a Shelly Flood S Gen4 sensor to detect rain and trigger
the Covers closed automatically. For that automation to close a window to a
sensible position (rather than just blindly commanding "closed" on a device
that may already be mid-travel), each Cover needs to have gone through the
full calibration routine in
[Shelly Configuration, Step 4](shelly-configuration.md#step-4-calibrate-travel-time)
so its reported open percentage is accurate — this calibration is the
remaining prerequisite before rain-triggered automation can be added.

## Per-window hardware cost

Because each motor requires its own Shelly 2PM Gen4 and OONO F-1020 (see
[Troubleshooting](troubleshooting.md#2-overcurrent-trip-from-wiring-two-motors-to-one-shelly-output)),
the per-window hardware cost is higher than a single shared controller
would be. For installations with many skylights, this adds up compared to
the original single-WLC-100-for-many-motors architecture.

## No WindowMaster protocol compatibility

This retrofit fully abandons the original WindowMaster digital keypad
protocol in favor of simple momentary switch wiring. This means original
WLI 130 keypads (which relied on that protocol) can't be reused as-is —
only their wiring runs are reused, with new plain pushbuttons substituted
at the wall. If you have other WindowMaster-protocol accessories you hoped
to keep using, they won't be compatible with this setup.

## Feedback welcome

This is a documented, working DIY solution for one real installation, not
an exhaustively engineered product. If you build on this and find
improvements — especially around limit switches or rain sensing — consider
contributing back via the project's GitHub repository.
