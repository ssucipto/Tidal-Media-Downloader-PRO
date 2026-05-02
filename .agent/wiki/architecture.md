# System Architecture
# Update monthly or when service boundaries change
last_verified: 2026-05-02

## System Map

```text
[User]
  │
  ▼
[WPF Shell (ShellViewModel)]
  ├── LoginView/ViewModel      — OAuth2 device-code or access-token auth
  ├── SearchView/ViewModel     — search Tidal catalog, show CoverCard grid
  ├── DownloadView/ViewModel   — active / complete / error task lists
  │     └── TaskViewModel      — one task = one Album/Playlist/Artist/Track/Video
  │           ├── TrackTask    — resolves StreamUrl → downloads audio → writes tags
  │           └── VideoTask    — resolves VideoStreamUrl → downloads segments → merges
  ├── SettingsView/ViewModel   — persists Settings.json
  └── AboutView/ViewModel      — version info, links

[TidalLib] ← all Tidal HTTP calls go here; never call Tidal directly
[AIGS]     ← file, http, encrypt, system helpers
[MediaToolkit/FFmpeg] ← video segment merging
```

## Service Boundaries

| Module | Owns | Does NOT own |
| --- | --- | --- |
| TidalLib | Auth, search, stream URL resolution, metadata | File I/O, UI state |
| TIDALDL_UI.Pages | ViewModel state, user actions, UI navigation | Download logic |
| TIDALDL_UI.Else | Download orchestration, file paths, config | Tidal HTTP calls |
| AIGS | Low-level helpers (HTTP, file, crypto) | Business logic |

## Key Data Flows

**Auth flow:**
`LoginViewModel.OnViewLoaded` → `Client.Login(accessToken)` → on success: show `MainView` + set `Global.AccessKey`

**Search flow:**
`SearchViewModel.Search` → `Client.Get(key, query, ...)` → returns `SearchResult` or direct entity → populate `CoverCards`

**Download flow:**
`SearchViewModel` → detail fetch → `DownloadViewModel.AddTask(detail)` → `TaskViewModel` constructor → creates `TrackTask`/`VideoTask` per item → each runs on `ThreadTool` thread pool → progress updates via `ProgressHelper`

**Settings persistence:**
`Settings` and `UserSettings` are JSON files in `~/Documents/Tidal-gui/data/`. `UserSettings` is base64-encoded via `EncryptHelper`. Read on startup, written on change.

## External Dependencies

| Service | Purpose | Access |
| --- | --- | --- |
| Tidal API | Streaming metadata + stream URLs | TidalLib (internal .dll) |
| GitHub API | Auto-update version check | HttpHelper (unauthenticated) |
| Local filesystem | Settings, downloads, cover art | AIGS FileHelper |
| MediaToolkit/FFmpeg | Video segment merging | Bundled binary |

## Platform & Build Requirements

**Platform: Windows only.** This project cannot be built or run on macOS or Linux.

| Blocker | Reason |
| --- | --- |
| WPF | Windows-only UI framework — no macOS support |
| .NET Framework 4.8 | Windows-only runtime (`net48`, not .NET Core/5+) |
| `OutputType: WinExe` | Compiled as a Windows executable |
| `Microsoft.Win32.*` / AlphaFS | Windows-specific APIs |
| MediaToolkit | Wraps a Windows FFmpeg binary |

**To build, you need (Windows only):**

- Visual Studio 2019 or 2022
- .NET Framework 4.8 SDK
- `TidalLib` sibling repo cloned and built at `../../TidalLib/` — referenced as `../../TidalLib/TidalLib/bin/Debug/TidalLib.dll`
- `AIGS` sibling repo cloned and built at `../../AIGS/` — referenced as `../../AIGS/bin/Debug/AIGS.dll`
- NuGet packages restored automatically from `packages.config`

⚠️ `TidalLib` and `AIGS` are **not included** in this repository. They must be obtained and built separately before the solution will compile.

**macOS alternative:** Use `tidal-dl` (CLI) — `pip3 install tidal-dl --upgrade` — supports Windows / Linux / macOS / Android.

