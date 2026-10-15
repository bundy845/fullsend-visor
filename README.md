# FullSend Visor

FullSend Visor brings an existing SimHub/Lovely dashboard into iRacing VR as an adjustable display inside the cockpit. Initial users are the project owner and one friend with matching Pimax Dream Air, Sboy3, and SteamVR setups.

## Status

Planning is approved. Implementation was started before the planning documents were written and is now paused for documentation review. The repository contains an initial .NET solution, placement/settings code, test source, and Windows project scaffolding. **There is no runnable application or verified VR integration yet.** Package restore succeeded; builds, tests, and headset checks have not yet been performed.

## Project documentation

- [Product](docs/product.md): users, scope, user journey, and acceptance criteria.
- [Architecture](docs/architecture.md): stack, rendering path, platform boundaries, and risks.
- [Roadmap](docs/roadmap.md): milestone order, deliverables, verification gates, and actual status.
- [Fixed dashboard specification](docs/features/fixed-dashboard.md): first-release behavior.
- [Verification](docs/verification.md): machine-specific checks and evidence ledger.
- [Design index](design/README.md): reference material and intended controls.
- [Repository instructions](AGENTS.md): commands, constraints, and definition of done.
- [Original handoff](docs/visor-planning.md): historical discovery context, superseded where the approved documents are more specific.

## Development setup

Install the .NET 10 SDK; `global.json` allows the latest installed .NET 10 feature band. Windows development targets x64 and Windows 10 build 19041 or newer APIs; use a supported Windows installation for the pilot.

From the repository root:

```sh
dotnet restore FullSend.Visor.slnx
dotnet test tests/FullSend.Visor.Core.Tests/FullSend.Visor.Core.Tests.csproj
dotnet build src/FullSend.Visor.Windows/FullSend.Visor.Windows.csproj
```

The test and build commands are the intended verification entry points, not claims that the current scaffold passes. The Windows application is incomplete. Cross-compilation may be possible from Mac, but Windows UI, capture, and VR behavior require Windows execution.

- Mac: portable core development, automated tests, documentation, and code review.
- Spare Windows desktop: UI, synthetic dashboard, capture, packaging, and failure handling.
- Racing PC: SimHub/Lovely, SteamVR, iRacing, headset placement, and frame-time testing.

## Planned operation and distribution

The application will open a user-supplied local SimHub dashboard URL in a dedicated preview window, then display it in SteamVR. Desktop controls will adjust placement and save one default. Existing SimHub wheel/button bindings continue to control Lovely.

The preview window must remain open for the prototype. Missing SteamVR must not prevent desktop preview. A built-in synthetic dashboard will allow testing without sim software.

The pilot will use a versioned, self-contained Windows x64 ZIP, with manual updates and a documented WebView2 Runtime prerequisite. Run/publish scripts and exact launch instructions will be added with the functioning application; none exist yet. Each user supplies their own SimHub/Lovely installation. Lovely assets are not redistributed.

Settings and diagnostic logs will be local to each user's application-data directory. There is no hosted service, account, or cloud synchronization.

## Git and remote

The existing repository uses `main` as its base branch and `origin` at `https://github.com/bundy845/fullsend-visor.git`. Current feature work is on `feature/vr-dashboard-prototype`. The implementation plan authorizes local development and verified milestone commits; remote changes and integration are not part of this task. Follow the user's active Git instructions for integration and cleanup.
