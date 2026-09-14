---
layout: page
title: Shelly Configuration
permalink: /docs/shelly-configuration/
nav_order: 6
icon: /docs/images/shelly-config-icon.jpg
description: >-
  Shelly app/web Cover mode setup walkthrough for skylight motor control,
  including why calibration fails on OONO relay-mediated wiring.
---

# Shelly App/Web Setup Walkthrough

Once a window is wired and you've confirmed the motor moves correctly in
both directions via the wall buttons (see [Build Guide](build-guide.md)),
configure the Shelly 2PM Gen4 as a **Cover** device so the app, wall
buttons, and any automations all work together correctly.

## Step 1: Add the device to the Shelly app

1. Power on the Shelly and follow the standard Shelly app pairing flow to
   add it to your WiFi network (or Zigbee/Matter hub, depending on your
   ecosystem).
2. Give it a clear name per window (e.g. "Skylight - Living Room") so you
   don't mix up channels later.

## Step 2: Switch the device profile to Cover

Cover mode is set from the Shelly device's own settings:

1. Open the device in the Shelly app and go to **Device Settings** by clicking the Shelly device.
2. Select the **Gear Configuration** blade from the left-hand navigation selector.
3. Under **Device Profile**, switch it from **Switch** to **Cover**.

While you're in this settings area, also set:

- **Input Type**: **Switch** The author's setup uses momentary rocker switches wired to S1/S2.  If using doorbell-type buttons (2 per window) you may need to use type "Button" setting.  
- **Movement Time Limits**: start conservatively at **15s** and re-verify
  after calibration (Step 4). Across the reference build's windows, actual
  calibrated times land around **16, 16.5, and 16.9 seconds**, so 15s is a
  safe starting ceiling that won't clip a real travel cycle once
  calibrated.
- **Reverse Directions**: because the motor leads landing on the OONO M1/M2
  terminals may not have a known polarity/orientation, enable **Reverse Directions** if open/close come out reversed — this avoids
  having to physically re-wire the motor leads to fix the direction.

![Shelly Cover device profile setting in the app](images/Shelly-Cover-Setting.JPEG)

## Step 3: Assign outputs and inputs

When prompted (or in the Cover component's channel settings), assign:

- **O1** and **O2** as the Cover's open/close outputs (these drive the OONO
  FWD/REV trigger inputs)
- **S1** and **S2** as the Cover's open/close inputs (these read your wall
  pushbuttons)

Double-check open maps to open and close maps to close — if they're
swapped, your wall button will close the skylight when you press "open."
This is easy to fix in software (see **Reverse Directions** in Step 2)
without re-wiring, so don't worry if you need to flip it after testing.

## Step 4: Set travel time (calibration will not succeed on this build)

> **Calibration will fail on this build, and that's expected.** This
> build's architecture routes the Shelly's O1/O2 outputs through an OONO
> F-1020 relay module before reaching the motor. The Shelly can only
> monitor power on its own output (the relay coil), not the actual motor
> current on the far side of the relay — and Shelly's calibration routine
> requires motor power monitoring to detect end-of-travel. Per Shelly's own
> documentation, calibration fails whenever the Cover controls a motor
> "via intermediate switches, contactors, etc." This is a permanent
> limitation of this design, not a configuration mistake — don't spend time
> troubleshooting a calibration failure here. See
> [Limitations](limitations.md#position-tracking-is-not-available-on-this-build)
> for the full explanation.

Because the Shelly's own calibration routine cannot succeed on this
topology, travel time has to be set manually instead:

1. Start a stopwatch (a second phone works well) the moment you send the
   **Open** command from the app, and stop it the moment the window reaches
   full travel. Repeat for **Close**.
2. Enter the measured time as the **Movement Time Limit** for that
   direction (see Step 2), padding slightly — the reference build's windows
   landed around **16, 16.5, and 16.9 seconds**, with the close time padded
   a bit "just to make sure."
3. This time limit is a blind timer cutoff, not a position measurement — the
   Shelly does not know where the window physically is partway through a
   move, only how long it's been running the motor.

> **Position percentage (0–100%) is not available on this build.** Only
> Open/Stop/Close commands work, each bounded by the Movement Time Limit
> set above. Any automation or dashboard expecting a Shelly-reported open
> percentage will not get one here.

> **Only run one window at a time.** Running more than one window's motor
> simultaneously — even when each has its own Shelly and OONO — has been
> observed to cause an overcurrent condition that stops both mid-travel
> (see [Troubleshooting](troubleshooting.md#2-overcurrent-trip-from-wiring-two-motors-to-one-shelly-output)).
> This applies when setting travel times and during normal use.

## Step 5: Understand how end-of-travel is detected

Because this retrofit bypasses the KEM 140's internal PCB (which used to
house the physical limit switches), the Shelly has no direct signal telling
it "fully open" or "fully closed." It relies on the **calibrated travel
time** from Step 4 to know when to stop driving the motor.

The Shelly 2PM Gen4 documentation describes built-in current-spike /
obstacle detection that should sense the motor stalling against its
mechanical end-stop and cut power. **In practice, on this build, that
detection has not been observed to trigger.** Instead, at end of travel the
motor keeps running against the mechanical stop until the calibrated time
elapses:

- Gear strain audibly builds (a whining sound) as the motor continues
  driving against the stop.
- The window mechanism clunks, as if the stop is being run over/rolled
  past.
- Strain releases briefly, then builds again as the motor keeps pushing.

This is harder on the gears and stop hardware than the current-spike
detection is supposed to allow, and it's a known limitation of the current
build — see [Limitations](limitations.md) for details and the planned fix
(external roller-lever micro limit switches).

Because there is no calibration and no position percentage (Step 4), **this
soft-cutoff behavior happens on every single Close command that runs the
full time limit** — not just as a rare edge case. This includes commands
sent automatically by an automation (e.g. closing on rain) with no human
present to notice the strain. See
[Limitations](limitations.md#no-rain-sensor-integration-yet) for why this
matters specifically for automated closes.

## Step 6: Test from the app and wall buttons together

1. Open and close the skylight from the app; confirm smooth full-travel
   movement in both directions and correct behavior of the Open/Stop/Close
   commands (there is no position percentage to check — see Step 4).
2. Test the physical wall buttons again now that Cover mode is configured,
   confirming they still behave correctly.
3. If you're integrating with a smart home platform (Matter, Zigbee, etc.),
   verify the Cover exposes as an expected shade/cover entity and that
   open/close/stop commands all behave as expected.

Once one window is fully configured and tested, repeat for the remaining
windows. When everything's running, review
[Troubleshooting](troubleshooting.md) and [Limitations](limitations.md) for
known pitfalls and what's still on the roadmap.
