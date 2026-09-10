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

## Step 2: Create a Cover component (the confusing part)

This is the step that trips almost everyone up: Cover mode is **not** found
where you'd expect, under Device Settings or Components.

1. From the Shelly app's **Home screen**, tap **Create new component**.
2. Select **Cover** from the component type list.
3. The app will walk you through assigning which physical outputs and
   inputs belong to this Cover component.

If you go looking for this under "Device Settings" or an existing
"Components" list, you won't find it — Cover components are created fresh
from the Home screen, not configured after the fact on an existing switch
component.

## Step 3: Assign outputs and inputs

When prompted, assign:

- **O1** and **O2** as the Cover's open/close outputs (these drive the OONO
  FWD/REV trigger inputs)
- **S1** and **S2** as the Cover's open/close inputs (these read your wall
  pushbuttons)

Double-check open maps to open and close maps to close — if they're
swapped, your wall button will close the skylight when you press "open."
This is easy to fix in software without re-wiring, so don't worry if you
need to flip it after testing.

## Step 4: Calibrate travel time

The Shelly's Cover mode uses a calibration routine to learn how long a full
open-to-close cycle takes, which it then uses to estimate position (0–100%)
between fully open and fully closed.

1. In the Cover component's settings, run the calibration/limit-setting
   routine (naming varies slightly by firmware version, look for
   "Calibrate" or "Set limits").
2. Let the motor run a full open and full close cycle uninterrupted during
   calibration.
3. Expect a measured travel time in the neighborhood of **16–17 seconds**,
   though this will vary slightly window to window depending on sash size
   and mechanical friction. Consistency between open and close times for the
   same window is a good sign of healthy mechanics.

## Step 5: Understand how end-of-travel is detected

Because this retrofit bypasses the KEM 140's internal PCB (which used to
house the physical limit switches), the Shelly has no direct signal telling
it "fully open" or "fully closed." Instead, it relies on:

- The **calibrated travel time** from Step 4, and
- Its **built-in current-spike / obstacle detection**, which senses the
  motor stalling against its mechanical end-stop and cuts power

This works, but it's a known limitation of the current build — see
[Limitations](limitations.md) for details and the planned fix (external
roller-lever micro limit switches).

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
