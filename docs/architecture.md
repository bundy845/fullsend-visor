# Architecture

## Approved approach

Use C#/.NET 10 for the application and portable logic; WPF for the Windows desktop controls; WebView2 for the dashboard preview; Windows Graphics Capture and Direct3D 11 for dashboard frames; Valve's OpenVR overlay interface for presentation through SteamVR.

```text
Existing SimHub/Lovely URL ─┐
                           ├─> WebView2 preview window
Synthetic test dashboard ──┘             |
                                Windows Graphics Capture
                                         |
                                Owned Direct3D 11 texture
                                         |
                                  SteamVR overlay
                                         |
                                 Dream Air / Sboy3

Desktop controls -> placement/settings -> overlay transform and width
```

This is the selected prototype path, not a claim of end-to-end validation. The first rig gate proves a static texture and placement; the next proves live capture.

## Project boundaries

`FullSend.Visor.Core` contains placement calculations, configuration validation, and persistence. It must run and be testable on Mac. `FullSend.Visor.Windows` owns WPF, WebView2, capture, Direct3D, OpenVR, lifecycle, and diagnostics. Keep these platform components cohesive; do not create a generalized backend/plugin framework.

A future car identifier input can select settings without replacing the renderer. A future steering transform can build on fixed placement. Neither requires a live telemetry dependency in the first version.

## Placement and coordinate conventions

Use metres internally for physical placement and degrees for user-facing rotation. OpenVR uses a right-handed coordinate system with negative Z forward. System.Numerics uses row-vector transforms; explicitly convert to OpenVR's matrix representation at the integration boundary.

“Place in front of me” takes the current tracked head position and horizontal heading, ignoring head pitch and roll. Save that anchor plus the user's placement offsets. The panel remains fixed in tracking space afterwards. Local panel rotation changes its orientation about its own centre; wheel-pivot rotation is future work.

Preserve aspect ratio from captured content dimensions. A saved tracking-space anchor can become inappropriate after recentering or room setup changes; provide explicit re-placement rather than promising automatic cockpit attachment.

## Dashboard and GPU lifecycle

The user supplies an HTTP(S) dashboard URL, or selects a bundled synthetic dashboard. Keep the preview open. Capture its content without desktop control chrome. Begin with a 30 FPS dashboard update target, independently of headset refresh.

Copy a captured frame into an application-owned Direct3D 11 texture before submitting to SteamVR. Release capture frames promptly. Manage texture lifetime, GPU adapter compatibility, resize, and disposal explicitly. Avoid CPU screenshot encoding as the steady-state render path.

No native game rendering hooks or game depth access are included. The overlay may remain visible over cockpit objects. OpenXR through SteamVR and OpenVR must be checked against the actual rig; direct Pimax OpenXR support is outside the first release.

## Configuration and failures

Store one default source/placement locally with a schema version. Validate finite transforms, sensible dimensions, and supported URL schemes. Write atomically; preserve the previous/original configuration on recovery. Do not persist runtime handles or credentials.

Desktop preview must work without SteamVR. Explain missing WebView2, invalid/unreachable dashboard, unavailable capture, closed/minimized preview, and disconnected SteamVR separately. Provide retry without requiring edits to configuration files. On capture failure, avoid silently presenting frozen content as live; communicate the state and recover or hide the overlay as appropriate.

## Packaging and development

Build a self-contained Windows x64 ZIP with manual updates, version identification, prerequisite checks, and bounded local logs. Keep WebView2 as an external runtime prerequisite. Include licenses for shipped dependencies. Do not bundle Lovely.

Mac supports core tests and source work. The spare Windows desktop supports UI/capture tests with synthetic content. The racing PC is required for VR and iRacing performance evidence. Windows cross-compilation from Mac does not substitute for runtime verification.

## Risks and unresolved validation

- Preview capture when covered/minimized and at different DPI settings.
- GPU adapter selection and capture-to-overlay texture synchronization.
- Correct image orientation, aspect ratio, readable resolution, and stereo distance.
- SteamVR reconnect and tracking-origin changes.
- CPU/GPU cost alongside iRacing; baseline measurements are still needed.
- No verified explanation of DanSuzuki's occlusion technique or compatibility with iRacing.
- SimHub car/class identifiers and transport remain a future investigation.

## Sources and dependency provenance

- [Valve OpenVR overlay documentation](https://github.com/ValveSoftware/openvr/wiki/IVROverlay_Overview)
- [Valve C# bindings](https://github.com/ValveSoftware/openvr/blob/master/headers/openvr_api.cs)
- [Windows capture sample](https://github.com/robmikh/Win32CaptureSample)
- [SimHub web access](https://github.com/SHWotever/SimHub/wiki/Troubleshoot-Dashstudio-Web-access)
- [SimHub plugin SDK](https://github.com/SHWotever/SimHub/wiki/Plugin-and-extensions-SDKs)
- [Sboy3 native SteamVR driver guide](https://eu.pimax.com/blogs/blogs/sboys3-native-steamvr-driver-setup-guide)

Initial scaffold vendors Valve's bindings and win64 runtime from commit `0924064316de3effbcd1acf1e309182a2deb1c05`, with its LICENSE. Initial package selections are WebView2 `1.0.3537.50` and Vortice.Direct3D11 `3.8.3`. Restore succeeded; integration has not been compiled or exercised.
