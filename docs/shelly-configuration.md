---
layout: page
title: Shelly Configuration
permalink: /docs/shelly-configuration/
nav_order: 6
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

Cover mode is set from the Shelly device's own settings, not from a
separate "create component" flow:

1. Open the device in the Shelly app and go to **Device Settings**.
2. Select the **Gear Configuration** blade from the left-hand nav selector.
3. Under **Device Profile**, switch it from **Switch** to **Cover**.

While you're in this settings area, also set:

- **Input Type**: **Switch** (not "Button") — the wall pushbuttons are
  wired as maintained/latching wall switches feeding S1/S2, not momentary
  buttons, so the Input Type must match.
- **Movement Time Limits**: start conservatively at **15s** and re-verify
  after calibration (Step 4). Across the reference build's windows, actual
  calibrated times land around **16, 16.5, and 16.9 seconds**, so 15s is a
  safe starting ceiling that won't clip a real travel cycle once
  calibrated.
- **Swap Inputs**: because the motor leads landing on the OONO M1/M2
  terminals may not have a known polarity/orientation, enable **Swap
  Inputs** if open/close come out reversed in software — this avoids
  having to physically re-wire the motor leads to fix the direction.

## Step 3: Assign outputs and inputs

When prompted, assign:

- **O1** and **O2** as the Cover's open/close outputs (these drive the OONO
  FWD/REV trigger inputs)
- **S1** and **S2** as the Cover's open/close inputs (these read your wall
  pushbuttons)

Double-check open maps to open and close maps to close — if they're
swapped, your wall button will close the skylight when you press "open."
This is easy to fix in software (see **Swap Inputs** in Step 2) without
re-wiring, so don't worry if you need to flip it after testing.

## Step 4: Calibrate travel time

The Shelly's Cover mode uses a calibration routine to learn how long a full
open-to-close cycle takes, which it then uses to estimate position (0–100%)
between fully open and fully closed.

1. In the Cover component's settings, run the calibration/limit-setting
   routine (naming varies slightly by firmware version, look for
   "Calibrate" or "Set limits").
2. Let the motor run a full open and full close cycle uninterrupted during
   calibration.
3. Expect a measured travel time in the neighborhood of **16–17 seconds**
   (the reference build's windows calibrated to 16s, 16.5s, and 16.9s),
   though this will vary slightly window to window depending on sash size
   and mechanical friction. Consistency between open and close times for the
   same window is a good sign of healthy mechanics.
4. Revisit the **Movement Time Limits** value from Step 2 once you know the
   real calibrated time for this window, so the limit isn't cutting travel
   short.

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

## Step 6: Test from the app and wall buttons together

1. Open and close the skylight from the app; confirm smooth full-travel
   movement and that the app's position indicator ends up at 0%/100% as
   expected.
2. Test the physical wall buttons again now that Cover mode is configured,
   confirming they still behave correctly.
3. If you're integrating with a smart home platform (Matter, Zigbee, etc.),
   verify the Cover exposes as an expected shade/cover entity and that
   open/close/stop commands all behave as expected.

Once one window is fully configured and tested, repeat for the remaining
windows. When everything's running, review
[Troubleshooting](troubleshooting.md) and [Limitations](limitations.md) for
known pitfalls and what's still on the roadmap.
