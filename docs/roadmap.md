# Roadmap and milestones

## Current position

**Current milestone: M0 — documentation baseline.** The approved plan is now recorded for review. Implementation is paused at the user's request while correcting the missing documentation.

Some M1 work was started prematurely: solution/project scaffolding, core placement/settings code, test source, and pinned OpenVR dependencies. Package restore succeeded. There is no running application, no build/test result, and no hardware validation. No implementation milestone is complete.

## M0 — documentation baseline

**Status:** Documentation written and local links checked; ready for user review. Implementation remains paused.

Deliverables: README, repository instructions, product scope, architecture, fixed-dashboard specification, reference notes, milestone roadmap, and verification ledger. Preserve the original handoff as historical context.

Exit criteria: documents consistently reflect approved decisions, distinguish implemented from planned behavior, have working local links, and identify hardware gates. Review the partial scaffold separately; do not treat its existence as a completed milestone.

## M1 — foundation and static VR proof

**Status:** Partial scaffold only; unverified.

Deliverables:

- Portable placement/settings logic with meaningful automated tests.
- Windows app that connects to SteamVR and displays a static test image.
- Adjustable position, distance, angle, and physical width; seated re-placement.
- Desktop operation without a headset for non-VR controls.

Exit criteria:

- Core tests and Windows build pass.
- On the rig, static content is visible in both eyes, correctly oriented and sized, has adjustable perceived distance, and stays fixed during head motion.
- Record actual iRacing mode, SteamVR/Sboy3 versions, and GPU. Confirm the overlay appears alongside iRacing.

**Hardware gate:** racing PC/headset. Do not invest in final UI polish before this path is proven. Commit completion only after required verification.

## M2 — live dashboard pipeline

**Status:** Not started. Depends on M1.

Deliverables:

- Dedicated WebView2 preview with synthetic animated dashboard and user-supplied URL.
- Windows Graphics Capture to owned Direct3D 11 texture to SteamVR overlay.
- Initial 30 FPS dashboard update target and preserved aspect ratio.
- Clear handling for loading, unavailable capture, resize, and preview closure/minimization.

Exit criteria:

- Animated content captures correctly on the spare Windows desktop.
- Live Lovely content appears readably on the rig; existing SimHub bindings still operate it.
- Capture/texture lifetimes remain stable through resize and reconnect exercises.
- Record baseline/on frame-time evidence; investigate repeatable degradation.

**Hardware gates:** Windows capture execution and racing rig live integration.

## M3 — usable personal prototype

**Status:** Not started. Depends on M2.

Deliverables: completed desktop placement controls; show/hide, reset, save/restore; default configuration; explicit re-placement after recentering; retry and diagnostic status; local logs.

Exit criteria:

- Complete setup/save/restart/race journey works on the owner's rig.
- Invalid settings recover without losing the original; missing dependencies give actionable errors.
- SteamVR stop/restart and dashboard failure can be recovered; no silently stale display.
- Relevant automated tests and manual verification matrix pass; operating docs match actual behavior.

## M4 — two-person pilot

**Status:** Not started. Depends on M3.

Deliverables: versioned self-contained Windows x64 ZIP, repeatable publish command/script, prerequisite checks, setup/update instructions, diagnostic collection instructions, and third-party notices.

Exit criteria:

- The package runs on the owner's and friend's matching setups without a development SDK.
- Each uses their own SimHub/Lovely installation and local settings.
- Readability, saved placement, recentering, reconnects, and performance are recorded for both rigs.
- Required checks pass and limitations are documented before marking the pilot ready.

Distribution is manual; no hosted deployment, auto-updater, or public launch is planned.

## Follow-up sequence

These are requested directions, not yet implementation-ready milestones:

1. Named manual placement profiles, then car/class assignment and automatic selection. Investigate SimHub identifiers/transport; use exact car → class → default with manual override.
2. Steering-following placement around a calibrated wheel pivot; investigate the steering input source and synchronization.
3. Cockpit occlusion feasibility: determine how to hide the dashboard behind the virtual wheel without assuming the existing overlay API provides game depth.
4. Suspension-related movement as a stretch goal: identify data and distinguish car motion from camera/head motion before choosing an approach.

Steering rotation, occlusion, and suspension work may require architectural changes. Write their specifications after feasibility research, before implementing them.
