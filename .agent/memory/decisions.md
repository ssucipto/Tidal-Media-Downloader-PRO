# Architecture Decision Records (ADR Log)
# Loaded by section (ADR ID) only — never fully loaded
# Add entries via /acp-decide command

## ADR-001: Use Stylet as MVVM Framework

- date: 2026-05-02
- status: accepted
- context: WPF desktop app needs a lightweight MVVM framework. Options: Stylet, MVVM Light, Prism, Caliburn.Micro.
- decision: Use Stylet — self-contained, no service-locator anti-pattern, Screen/Conductor lifecycle, clean BootstrapperBase integration with WPF startup.
- consequences:
  - All ViewModels inherit from `Stylet.Screen` or `ModelBase`.
  - Window/dialog management goes through `IWindowManager`; never instantiate windows directly.
  - `BindableCollection` replaces `ObservableCollection` for UI-thread-safe dispatch.
  - Do NOT introduce a second IoC container.

## ADR-002: Ship as Single Self-Contained EXE via Costura.Fody

- date: 2026-05-02
- status: accepted
- context: End-users are non-technical; a single .exe with no installer or dependency folder is required.
- decision: Use Costura.Fody to embed all referenced assemblies as compressed resources inside the output .exe at build time.
- consequences:
  - No loose DLLs shipped — output is a single `tidal-gui.exe`.
  - FFmpeg binary (MediaToolkit) must be bundled or extracted at runtime.
  - Do NOT strip Costura in debug builds unless testing embedding issues.
  - Version updates replace the single .exe via `update.bat`.

## ADR-003: TidalLib as Bundled Internal Assembly (not NuGet)

- date: 2026-05-02
- status: accepted
- context: TidalLib contains Tidal API reverse-engineering and proprietary client credentials that cannot be published as a NuGet package.
- decision: Reference TidalLib as a local .dll assembly. All Tidal API calls go exclusively through `TidalLib.Client.*` static async methods.
- consequences:
  - TidalLib updates require replacing the .dll in References and rebuilding.
  - New Tidal endpoints must wait for a TidalLib update — do not call Tidal HTTP directly from UI code.
  - Public API surface: `Client.Login`, `Client.RefreshAccessToken`, `Client.GetDeviceCode`, `Client.Get`.
