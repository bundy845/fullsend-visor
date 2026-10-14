# FullSend Dashboard / VR Project Handoff

**Status:** Early planning  
**Working name:** FullSend Visor (not final)  
**Intended reader:** Zed coding agent

## Project intent

Build a FullSend-branded application that initially adds **VR/3D presentation support to an existing sim racing dashboard**. This is **not** initially a telemetry reader, telemetry analyzer, or replacement dashboard.

The architecture should leave room for later features, potentially including desktop overlays, custom widgets, other dashboard integrations, and live iRacing telemetry, without requiring them in the first release.

## Existing FullSend ecosystem

There is an existing .NET 10 FullSend repository structured approximately as follows (build output omitted):

```text
FullSend/
├── docs/
├── samples/
│   ├── expected/
│   └── inputs/
├── src/
│   ├── fullsend-api/
│   ├── fullsend-diagnostics/
│   ├── fullsend-pulse/
│   └── fullsend-telemetry/
└── tests/
    ├── fullsend-api/
    ├── fullsend-pulse/
    └── fullsend-telemetry/
```

The existing FullSend work focuses on telemetry-related functionality, including reading/processing recorded iRacing `.ibt` files. Do **not** assume a live telemetry API already exists.

## Repository decision

**Current recommendation:** Start a separate repository for this product, tentatively `fullsend-visor`, while keeping it part of the FullSend brand/ecosystem.

Reasons:
- The first feature set is dashboard integration and 3D/VR presentation, not telemetry processing.
- It may have different runtime dependencies, build and release cycles, and deployment requirements from the existing .NET services/libraries.
- Shared telemetry functionality could be consumed later through a versioned library/package or an API, if justified.

This is a recommendation rather than a hard technical constraint. Do not create a shared repository or package until code is genuinely shared.

## Initial scope

1. Integrate with an **existing dashboard** (exact dashboard and integration method still need to be established).
2. Present that dashboard in **VR with 3D support** (exact meaning of 3D, headset/runtime, and rendering approach still need discovery).
3. Keep configuration and dashboard integration isolated from presentation/runtime-specific code.
4. Deliver an independently installable and releasable product.

### Explicitly out of scope for the first iteration

- Building a live iRacing telemetry reader.
- Reading recorded `.ibt` telemetry to drive the display.
- Implementing a full original dashboard or widget ecosystem.
- Creating a generic plugin system or shared FullSend packages before there is a demonstrated need.

## Possible future capabilities (not commitments)

- Live telemetry from iRacing.
- Flat-screen/desktop dashboard presentation and overlays.
- Custom widgets and layouts.
- Additional external dashboard integrations.
- Reuse of code from the existing `fullsend-telemetry` library.

## Architecture direction

Keep three concerns conceptually separate:

```text
Dashboard / data integration
          |
          v
Presentation model / configuration
          |
          v
Presentation backends
  ├── VR / 3D (initial)
  └── Desktop / overlay (possible future)
```

These are conceptual boundaries, **not** instructions to immediately create multiple assemblies or an elaborate abstraction framework. Start with the simplest implementation that fits the actual dashboard and VR runtime.

If live telemetry becomes relevant later, consider it another input capability and evaluate whether the existing FullSend telemetry code can be reused. Do not make this first implementation depend on `fullsend-pulse` or a telemetry service solely for hypothetical future needs.

## Naming

- **FullSend** is the umbrella brand.
- **FullSend Visor** is a working product name, fitting the VR-first release.
- If broader non-VR display features become central, alternatives previously discussed include **FullSend Dash**, **FullSend Display**, or **FullSend Studio**.
- Existing projects use lowercase, hyphenated repository/project directory names (`fullsend-telemetry`, etc.).

## Discovery questions for the coding agent

Before choosing frameworks or generating implementation code, determine:

1. Which **existing dashboard** is being integrated? Is it a web page, native window, texture, or another surface?
2. Does “3D support” mean placing an existing 2D dashboard in VR space, rendering stereo/depth-aware elements, or both?
3. Which VR hardware/runtime and sim environment must be supported (e.g., OpenXR, SteamVR, Windows)?
4. How is the dashboard accessed or captured, and are there performance, transparency, interaction, or compatibility requirements?
5. What is the smallest meaningful proof of concept?
6. Is .NET 10 appropriate for the implementation, or should a native/graphics-specific component be used?

## Requested next action

**Begin with discovery and an implementation proposal, not a large generated codebase.** Identify the smallest VR dashboard proof of concept, likely technology options, important technical risks, and a recommended repository structure based on the answers to the discovery questions. Clearly separate confirmed requirements from assumptions.
