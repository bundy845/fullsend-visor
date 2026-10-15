# FullSend Visor repository instructions

## Read first

Read `docs/product.md`, `docs/architecture.md`, `docs/roadmap.md`, and the specification for the milestone being implemented. Check `docs/verification.md` for actual evidence before claiming a feature is verified. `docs/visor-planning.md` is historical context.

## Layout and commands

- `src/FullSend.Visor.Core`: portable .NET 10 placement and configuration logic.
- `src/FullSend.Visor.Windows`: Windows x64 WPF application, dashboard rendering, GPU capture, and SteamVR integration.
- `tests/FullSend.Visor.Core.Tests`: portable xUnit tests.
- `third_party/openvr`: pinned Valve bindings, native library, and license.

```sh
dotnet restore FullSend.Visor.slnx
dotnet test tests/FullSend.Visor.Core.Tests/FullSend.Visor.Core.Tests.csproj
dotnet build src/FullSend.Visor.Windows/FullSend.Visor.Windows.csproj
```

These are verification entry points. The initial Windows scaffold is incomplete and does not yet provide a runnable application. Do not describe restore success as a successful build or test run.

## Constraints

- Keep the core free of WPF, Windows capture, and OpenVR dependencies so it can be tested on Mac.
- Target SteamVR for the first release. iRacing's actual OpenVR/OpenXR mode remains a rig compatibility check.
- Reuse the user's existing SimHub dashboard and controls. Do not add a live telemetry reader, shared FullSend package, generic plugin framework, or cloud service for the first release.
- Use owned Direct3D textures for overlay submission; manage frame/device/session lifetimes explicitly.
- Keep dashboard URLs and machine-specific settings out of source control and diagnostic logs where possible.
- Do not redistribute Lovely dashboard content. Preserve third-party licenses and document dependency provenance.
- Do not overwrite invalid settings silently; provide recovery while retaining the original.
- Record reference screenshots as inspiration. Do not infer DanSuzuki's implementation from appearance.

## Definition of done

A milestone requires its documented acceptance criteria, relevant automated checks, appropriate Windows/rig checks, diff review, and synchronized documentation. Record unavailable hardware checks as pending, not passed. Do not mark a milestone complete or create its completion commit while required gates are outstanding.

When a hardware gate blocks verification, report the implemented portion, exact test procedure, and remaining evidence needed. Leave the roadmap truthful. No specialist agents are required for this project.
