# Correction Log — Filtered by task_type before loading
# Populated automatically when developer says "log it" or "wrong, log this"
# Max 5 entries loaded per session, filtered to current task_type + priority:high

- date: 2026-05-03
  task_type: architecture-design
  mistake: Assumed project could be built on macOS.
  correction: >-
    This project is Windows-only. WPF + .NET Framework 4.8 cannot run on macOS.
    Never suggest macOS build steps. For Mac users, redirect to tidal-dl CLI.
  priority: high

- date: 2026-05-03
  task_type: architecture-design
  mistake: Assumed TidalLib and AIGS are bundled inside this repository.
  correction: >-
    TidalLib and AIGS are separate sibling repositories not included here.
    They must be cloned and built at ../../TidalLib/ and ../../AIGS/ before
    the solution compiles. Always check for missing sibling deps before
    suggesting build steps.
  priority: high

