---
layout: page
title: Limitations
permalink: /docs/limitations/
nav_order: 8
icon: /docs/images/limitations-icon.jpg
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

**Current mitigation:** a manually-timed **Movement Time Limit** (measured
with a stopwatch, ~16–17 seconds — see
[Shelly Configuration, Step 4](shelly-configuration.md#step-4-set-travel-time-calibration-will-not-succeed-on-this-build))
acts as a blind timer cutoff. The Shelly's built-in current-spike/obstacle
detection, which is supposed to sense the motor stalling against its
mechanical end-stop and cut power early, **has not been observed to trigger
on this build** (see
[Shelly Configuration, Step 5](shelly-configuration.md#step-5-understand-how-end-of-travel-is-detected)).

This is a soft cutoff based purely on elapsed time, not a hard
electrical/mechanical limit and not current-sensing as originally hoped.
On every close that reaches the mechanical stop before the timer elapses,
the motor continues driving against that stop for the remainder of the
timed duration, straining the rack-and-pinion mechanism (audible gear
whine, a clunk, and repeated strain/release cycling).

**Planned improvement:** install external roller-lever micro limit
switches at each sash's fully-open and fully-closed positions, wired to
provide a hard cutoff independent of the Shelly's timing/current logic.
This is not yet implemented in this build. As covered below, this
improvement is also necessary to protect against unconditional automated
Close commands, not just manual operation.

## Position tracking is not available on this build

Shelly Cover calibration — and the 0–100% position reporting it enables —
cannot succeed on this build. Calibration relies on the Shelly monitoring
the **motor's own power draw** to detect when it hits an end stop. In this
build, the Shelly's O1/O2 outputs don't drive the KEM 140 motor directly —
they trigger the **OONO F-1020 relay module**, which then switches power to
the motor. The Shelly can only see current on its own output (the OONO
relay coil, a few milliamps), never the actual motor current on the far
side of the relay. Per Shelly's own documentation, calibration fails
whenever the Cover controls a motor "via intermediate switches, contactors,
etc." — exactly this build's topology.

**No amount of retrying, adjusting Movement Time Limits, or re-wiring will
make calibration succeed.** This is a structural limitation of any build
using an external polarity-reversal relay (OONO or otherwise) between a
Shelly Cover and a DC gearmotor, not something specific to this
installation's wiring or settings. Anyone replicating this design with a
similar relay-mediated architecture should expect the same limitation.

The practical consequence: there is no 0–100% position readout on this
build, ever. Only Open/Stop/Close commands work, each bounded by a manually
set Movement Time Limit acting as a blind timer cutoff (see
[Shelly Configuration, Step 4](shelly-configuration.md#step-4-set-travel-time-calibration-will-not-succeed-on-this-build)).

## No rain sensor integration (yet)

The original WindowMaster system supported a rain sensor input (the `6`
terminal on the WLC 100) that could automatically close skylights when
precipitation was detected. This retrofit does not currently replicate that
functionality, and doing so safely on this build is less simple than it
first appears.

**Rain automation must work without position data.** Since calibration
cannot succeed (see above), any rain-triggered close automation has no
Shelly-reported percentage to check — it can only send an unconditional
**Close** command to every window.

**Unconditional Close is not risk-free.** Sending Close to a window that is
already closed is not a harmless no-op: because there is no position
feedback, the Shelly cannot know the window is already shut, so it runs the
motor for the full Movement Time Limit duration regardless, driving it
against the mechanical end-stop for the entire ~15–20 second window. This
produces the same audible gear whine, clunk, and strain/release cycling
described in
[Shelly Configuration, Step 5](shelly-configuration.md#step-5-understand-how-end-of-travel-is-detected)
— and doing this automatically, potentially every time it rains, is a real
mechanical-wear concern, not just a theoretical one.

**Planned mitigation: gate automation on an external sensor.** A Shelly BLU
Door/Window sensor mounted on each sash can report true open/closed state
independent of the Shelly Cover or the OONO relay. The rain automation
should check this sensor before sending Close, and skip any window that's
already shut. This makes the BLU sensor a **required** component for safe
rain automation on this build, not an optional accuracy nice-to-have.

**Sensor and limit switches are complementary, not redundant.** The BLU
sensor fixes the *automation's* behavior (don't send Close unnecessarily).
The external roller-lever limit switches planned above fix the *motor's*
exposure regardless of what commanded the close — human error, a future
automation bug, or any other source. Both are needed to close out this gap
safely, not the limit switches alone.

See also the note on running multiple windows simultaneously in
[Troubleshooting](troubleshooting.md#2-overcurrent-trip-from-wiring-two-motors-to-one-shelly-output) —
any rain-close automation covering more than one window will still need to
sequence windows one at a time rather than commanding them all closed at
once.

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
