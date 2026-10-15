# Fixed dashboard specification

## User journey and controls

The user opens Visor, chooses synthetic content or enters a SimHub dashboard URL, verifies the desktop preview, connects to SteamVR, and places the display near the virtual wheel. Controls operate while parked from the desktop; VR controller interaction is excluded.

Provide horizontal/vertical movement, distance, pitch/yaw/roll, and physical width. Preserve aspect ratio. Provide Place in front of me, Show/hide, Reset placement, Save placement, Restore saved placement, and connection retry. Controls must have labels, units, keyboard access, and readable status text.

The placement anchor is fixed after calibration. Head movement must change perspective without moving the dashboard. Reset restores usable placement offsets; Place in front of me establishes a new anchor from the current seated pose. Do not automatically follow the head.

## Sources and preview

Accept an HTTP(S) dashboard URL and provide bundled synthetic animated content. Empty source selection opens the synthetic option or guides the user to it. An invalid address produces a visible error without crashing. Use the user's existing SimHub/Lovely installation and bindings; Visor does not own dashboard page switching.

Keep the preview window open. Capture dashboard content rather than configuration controls. A closed or minimized preview is a supported failure state with a restore/retry instruction; background rendering is not guaranteed.

## Persistence

Save one default source and placement locally. A fresh installation starts with sensible placement offsets and synthetic content available. Restore the saved configuration across application restarts. Explicitly saving is required to replace the saved placement. Invalid or unsupported settings load defaults with a warning and preserve the original for recovery.

Tracking-space coordinates can change after recentering or room setup changes. Provide explicit re-placement and describe its use; do not claim permanent attachment to iRacing cockpit geometry.

## Runtime states

- Desktop only: preview and settings work; VR controls explain that SteamVR is unavailable.
- Ready: content is available and the overlay can be shown.
- Loading/unavailable source: communicate the problem and provide retry.
- Capture interrupted: communicate loss and avoid leaving an apparently live frozen panel.
- VR disconnected: retain configuration and desktop preview; provide reconnect.
- Missing WebView2: give installation guidance rather than an unhandled exception.

Logs should identify version, component failures, and recovery without routinely recording dashboard URLs or telemetry. Keep logs local and bounded.

## Acceptance scenarios

Use the machine-specific checklist in [verification](../verification.md). First release is complete only after a live dashboard works on both matching pilot rigs with saved placement, recovery behavior, and measured performance.

This feature does not promise cockpit occlusion, a 3D bezel, wheel rotation, suspension movement, or car-based profile selection. Its flat surface must nevertheless have adjustable physical depth and correct stereo perspective.
