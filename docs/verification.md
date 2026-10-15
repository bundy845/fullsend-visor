# Verification plan and evidence

## Evidence ledger

| Check | Current evidence |
| --- | --- |
| Repository inspection | Existing Git repository; original handoff committed on main; no unrelated changes found before work |
| Feature branch | `feature/vr-dashboard-prototype` created |
| Development SDK | Mac has .NET SDK 10.0.401 |
| Dependency restore | `dotnet restore FullSend.Visor.slnx` succeeded for all three projects |
| Documentation | Local links checked across 10 Markdown files; all resolve; tracked diff whitespace check passed |
| Core tests | Test source exists; not run |
| Windows compilation | Not run; scaffold is incomplete |
| Windows UI and capture | Not implemented/verified |
| SteamVR and headset | Not implemented/verified |
| iRacing/Lovely | Not implemented/verified |
| Friend pilot package | Not produced |

Restore success is not evidence of build success. Written tests are not evidence of passing tests. Update this table with commands, dates, machine details, and results as verification happens.

## Mac checks

Run core tests for placement transforms, horizontal head anchoring, rotation about the panel centre, aspect ratio, finite/range validation, settings round trips, invalid settings preservation, unsupported schema, and invalid source addresses. Review diffs and documentation links.

## Spare Windows desktop checks

Build/run the Windows application using synthetic content without sim software or a headset. Verify UI keyboard access, preview loading, capture frame updates, content-only capture, resize and DPI changes, preview covered/minimized/closed behavior, prerequisite errors, save/restore, and clear desktop-only status.

Run the packaged application without the SDK. Confirm dependency notices, diagnostic paths, and manual update instructions.

## Racing rig checks

Record Windows/GPU, headset, Sboy3, SteamVR, SimHub, Lovely, and iRacing display mode. Check static content before live content.

- Both eyes display correctly, with legible text, correct orientation/aspect ratio, and adjustable perceived distance.
- Head translation and rotation leave the panel fixed while perspective changes.
- All desktop placement controls work and save/restart restores them.
- Re-placement after recentering returns the panel to a useful position.
- Live Lovely updates and existing SimHub bindings continue to work.
- Stop/restart SteamVR, interrupt dashboard loading, and close/minimize preview; verify status and recovery without stale content presented as live.
- Exercise hide/show and exit; no stranded overlay or lingering application process.

## Performance comparison

Use comparable sessions, cars, graphics settings, headset refresh, and driving conditions. Record CPU/GPU frame times and dropped/reprojected frames with Visor off and on, plus dashboard update behavior. Repeat an apparent regression to distinguish noise. Establish a measured budget with the owner before declaring performance acceptable for the friend pilot; do not invent a passed threshold.

## Friend pilot

Repeat setup, readability, placement persistence, recentering, reconnect, and performance checks using the ZIP on the friend's matching rig. Record differences and package version. A clean build or passing core tests alone cannot close M4.
