# Service Integrations — auto-populated by acp-init (2025-05-02)

XML-tagged sections; load one section at a time.

<tidal_api>
  name: Tidal Streaming API
  type: music-streaming REST API
  accessed_via: TidalLib (bundled C# library)
  auth_flows:
    - device_code: user visits verification URL, polls until authorized
    - access_token: Bearer token stored in UserSettings.Accesstoken
    - refresh_token: stored in UserSettings.Refreshtoken (auto-refresh on expiry)
  credentials:
    - key: Accesstoken     # value stored in: PATH_USERSETTINGS (base64-encoded JSON)
    - key: Refreshtoken    # value stored in: PATH_USERSETTINGS (base64-encoded JSON)
    - key: Sessionid1      # legacy session-based auth (older flow)
    - key: Sessionid2      # legacy session-based auth (older flow)
  config_refs:
    - Global.CommonKey  — primary LoginKey for search/metadata
    - Global.VideoKey   — LoginKey used for video stream URLs
    - Global.AccessKey  — OAuth2 LoginKey (preferred when set)
  notes: Country code (Countrycode) determines available content; HiFi/MQA quality gated by subscription tier
</tidal_api>

<github_api>
  name: GitHub REST API
  type: version-check / update
  base_url: <https://api.github.com>
  usage: fetch latest release of yaronzz/Tidal-Media-Downloader-PRO for auto-update
  env_vars: none (unauthenticated public API calls)
  config_refs:
    - Global.NAME_GITHUB_AUTHOR   = "yaronzz"
    - Global.NAME_GITHUB_PROJECT  = "Tidal-Media-Downloader-PRO"
    - Global.NAME_GITHUB_FILE     = "tidal-gui.exe"
    - Global.PATH_UPDATE / PATH_UPDATEBAT — local update staging paths
</github_api>

<genius_api>
  name: Genius
  type: lyrics metadata API
  accessed_via: Genius.NET NuGet package (v4.0.1)
  usage: enrich downloaded tracks with lyrics/annotations (optional feature)
  env_vars: GENIUS_CLIENT_ACCESS_TOKEN  # value not found in source — may be hardcoded in TidalLib or unused
</genius_api>

<local_filesystem>
  name: Local File System
  type: storage
  paths:
    - settings_dir:    ~/Documents/Tidal-gui/data/
    - settings_file:   settings.json      (plain JSON — app settings)
    - usersettings:    usersettings.json  (base64 + EncryptHelper — credentials)
    - update_dir:      ~/Documents/Tidal-gui/update/
    - output_default:  ./download/
  config_key: Global.KEY_BASE = "fa^8jk@j9d"  # symmetric encode key for usersettings
</local_filesystem>

<media_processing>
  name: MediaToolkit / FFmpeg
  type: local media processing
  accessed_via: MediaToolkit NuGet package (v1.1.0.1)
  usage: merge/convert video segments after download (e.g. .ts → .mp4)
  env_vars: none (bundled FFmpeg binary)
</media_processing>
