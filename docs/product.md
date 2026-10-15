# Product: FullSend Visor

## Goal and users

Make the driver's existing Lovely Dashboard readable in iRacing VR, positioned like an instrument inside the cockpit. Begin on the owner's rig and quickly validate on one friend's matching setup. FullSend Visor is a separate application/repository within the FullSend ecosystem.

## Confirmed environment and preferences

- Pimax Dream Air connected by USB and DisplayPort, using Sboy3 and SteamVR.
- Pimax OpenXR is not in use. iRacing's selected OpenVR/OpenXR mode will be confirmed during rig testing.
- Existing SimHub and Lovely installations supply the dashboard and telemetry behavior.
- Desktop placement controls are sufficient. Existing SimHub bindings control dashboard pages/actions.
- An open desktop preview window is acceptable for the prototype.
- Development should be possible on Mac, with a spare Windows desktop and racing PC used for appropriate validation.
- First useful result is fixed placement. Steering rotation follows; suspension movement is a stretch goal.
- Car/car-class placements are requested after the first version. SimHub is the preferred source to investigate for identification.

## First-release journey

1. Open Visor on Windows; choose the synthetic dashboard or enter the SimHub dashboard URL.
2. Confirm the dashboard is visible in the desktop preview.
3. Connect to SteamVR and place the display in front of the seated driver.
4. Adjust horizontal/vertical position, distance, pitch/yaw/roll, and physical width from desktop controls.
5. Save the placement; race using existing SimHub controls.
6. Restore that placement on the next launch, with a re-placement action if tracking origin or seating changes.

The same preview and configuration journey works without a connected headset, up to the VR-specific actions.

## First-release boundary

One opaque rectangular dashboard surface, correct perceived VR distance and head-motion perspective, fixed tracking-space placement, one saved default, show/hide, reset, retry, and local diagnostics. The driver manually starts and updates Visor.

Excluded: steering rotation, cockpit occlusion, suspension movement, automatic car detection, multiple dashboards, VR-controller manipulation, 3D housing, guaranteed minimized/background rendering, original dashboard widgets, recorded `.ibt` playback, a live telemetry reader, accounts, cloud synchronization, and non-SteamVR runtime support.

Depth means placement at a physical distance in the headset. Occlusion means cockpit objects hiding the display. The user explicitly accepted deferring occlusion even if the prototype draws over the wheel where they overlap.

## Acceptance criteria

- Live Lovely content is readable in both eyes at an adjustable distance.
- Looking around changes perspective without dragging the panel with the head.
- Desktop controls change location, orientation, and width while preserving content aspect ratio.
- Save/restart restores the selected source and placement; reset provides a usable default.
- Re-placement restores a useful position after recentering or a change in seating position.
- Missing SteamVR leaves desktop preview usable. Missing dashboard/capture prerequisites show an actionable status and retry path.
- Invalid configuration falls back safely and preserves the original file.
- A versioned pilot package works on both matching rigs, with instructions and local diagnostics.
- Comparable racing sessions establish the performance cost before the friend pilot. A numerical budget will be set from baseline measurements; no performance threshold has yet been agreed.

## Data and distribution

Each user owns their settings and uses their own SimHub/Lovely installation. Store configuration and bounded diagnostic logs locally under user application data. Avoid logging dashboard URL query strings or telemetry payloads. No upload, account, or online service is required by Visor. The user-supplied dashboard may retain its own network behavior.

Pilot distribution is a portable, self-contained Windows x64 ZIP with manual updates and WebView2 Runtime as a documented prerequisite. No budget, deadline, or public distribution commitment has been set.

## Follow-up capabilities

Named manual profiles precede automatic selection. Proposed precedence: exact car, then car class, then default, with manual override. Confirm stable SimHub identifiers and an appropriate integration path before implementing detection. Cars in the same class can require different positions.

Investigate steering rotation around a calibrated pivot, then cockpit occlusion and suspension-related motion. These are separate feasibility tasks, not promised behavior of the first renderer.
