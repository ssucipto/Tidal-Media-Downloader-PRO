# Session Memory
# Format: YAML blocks, last 3 loaded per session, auto-compacted at 15 entries
# DO NOT edit manually — updated by /acp-commit

- date: 2026-05-02
  executor: fast-model
  tasks: [INIT-001]
  done:
    - domain-extraction-domain-yml
    - integrations-md-populated
    - identity-yml-filled
    - agents-md-who-you-are-filled
    - three-adrs-written
    - taxonomy-yml-project-specific
    - architecture-md-populated
    - skills-download-md-filled
    - skills-ui-md-created
  deferred: {}
  key_fact: >
    TidalLib is a bundled internal .dll (not NuGet). All Tidal API calls go through
    TidalLib.Client.* static async methods. Do NOT call Tidal HTTP endpoints directly.

- date: 2026-05-03
  executor: fast-model
  tasks: [STATUS-001, REQ-001]
  done:
    - acp-status-ran-no-active-tasks
    - platform-requirements-checked
    - confirmed-windows-only-wpf-net48
    - confirmed-tidallib-aigs-are-missing-sibling-repos
    - architecture-md-build-section-added
    - lessons-md-windows-only-recorded-priority-high
    - lessons-md-missing-sibling-deps-recorded-priority-high
  deferred: {}
  key_fact: >-
    Project is Windows-only (WPF + .NET 4.8). TidalLib and AIGS are sibling repos
    not included here — must be built at ../../TidalLib/ and ../../AIGS/ to compile.
    Mac users must use tidal-dl CLI instead.
