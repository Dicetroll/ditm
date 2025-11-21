<!-- Project: DITM (demon-in-the-mirror) - Copilot instructions for AI coding agents -->
# Copilot / AI Agent Instructions — DITM (demon-in-the-mirror)

Purpose
- Help AI coding agents become productive quickly in this repository by documenting the architecture, conventions, and developer workflows that are discoverable in source files.

Big picture
- This repository is a Foundry VTT module (see `module.json`). The module distributes a set of "packs" (compendia) under `packs/` such as `packs/ditm-actors`, `packs/ditm-items`, and `packs/ditm-adventures`.
- Runtime: integrates with Foundry Virtual Tabletop and the `dnd5e` system. The module is not a standalone server app — it is packaged and consumed by a Foundry instance.

Key files & locations
- `module.json`: single-source manifest for the module (id, title, version, compatibility, `packs`, `manifest` and `download` URLs). Update this when releasing.
- `packs/<pack-name>`: compendium packs (one directory per compendium listed in `module.json`). Expect JSON/DB blobs and static assets here.
- Releases: `module.json` references GitHub release assets (`manifest` and `download` fields). Releases host `module.json` and a `module.zip` for Foundry to download.

Project-specific conventions
- Pack naming: compendia use the `ditm-` prefix and map to Foundry entity types. Example: `packs/ditm-items` -> `type: "Item"` in `module.json`.
- Ownership flags: packs include a consistent ownership mapping pattern. Example from `module.json`:

  - `"ownership": { "PLAYER": "OBSERVER", "ASSISTANT": "OWNER" }`

  Treat these fields as authoritative for default permissions when editing or creating pack entries.
- `system`: set to `dnd5e` in `module.json`. Expect content shaped for that game system.

Developer workflows (what to do when changing code/content)
- Bumping versions & releasing:
  - Update `version` in `module.json`.
  - Build a `module.zip` (zip root should match Foundry module layout), create a GitHub Release, upload `module.zip`, and update `manifest` and `download` URLs in `module.json` to point to the release assets.
  - Keep `compatibility.minimum/verified/maximum` in `module.json` accurate for target Foundry versions.
- Content changes (compendia updates): modify files under `packs/<pack-name>`, then include the updated pack files in the release `module.zip`.

Debugging & testing
- There are no automated tests or build scripts in the current snapshot. Typical debug workflow:
  - Run a local Foundry instance and install the module via the module manifest URL.
  - Use Foundry's browser devtools console for runtime errors and stack traces (client-side JS environment).
  - Inspect compendia content by opening pack directories or the Foundry UI.

Integrations & external dependencies
- Foundry VTT core and the `dnd5e` system are primary runtime dependencies; they are not declared in this repo but expected by the module's `system` field.
- Distribution uses GitHub Releases (manifest/download URLs). No package manager or CI scripts were found in the current repository snapshot.

How to contribute code changes (agent guidance)
- Make minimal, focused edits and keep `module.json` in sync for any change that affects runtime (new packs, different ownership, version changes).
- When creating a new pack, follow the naming convention `ditm-<purpose>` and add an entry in `module.json` `packs` array with `name`, `label`, `path`, `type`, `ownership`, and `system`.

Examples (copyable snippets)
- module.json pack entry:

  ```json
  {
    "name": "ditm-items",
    "label": "DITM-Items",
    "path": "packs/ditm-items",
    "type": "Item",
    "ownership": { "PLAYER": "OBSERVER", "ASSISTANT": "OWNER" },
    "system": "dnd5e"
  }
  ```

Limitations & notes for agents
- The current repository snapshot contains only the manifest (`module.json`). Do not assume build scripts, tests, or CI configs exist unless you find them.
- Make changes only to discoverable, repository-contained artifacts (files under the repo). Do not publish releases or update remote URLs without explicit user instruction.

If unsure, ask
- Ask the human maintainer for:
  - Where `module.zip` artifacts are built (if there is a private build script).
  - Intended release process and whether the manifest should be updated automatically.
  - Any hidden conventions for naming content or pack structure not present in `module.json`.

End of instructions.
