# Hyura Mod Manager (HMM)

A desktop mod manager for Windows that installs, deploys and organizes mods for games
built on **RE Engine**, **MT Framework** and **Unreal Engine**.

HMM keeps every mod in its own folder, deploys only what you enable, backs up the
original game files, and restores them when you disable or uninstall a mod. It
understands Fluffy `modinfo.ini` packages, REFramework plugins, Unreal `.pak` and
LogicMods files, loose Lua/DLL mods, plain overwrite mods, and Hyura packages.

![Hyura Mod Manager managing mods for Spore: Galactic Adventures](docs/mod-manager.png)

<p align="center">
  <a href="https://github.com/TaylorHyura/Hyura-Mod-Manager/releases">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-2ea44f?style=for-the-badge" alt="Download the latest release">
  </a>
</p>

> **Status: pre-release.** HMM is under active development. The first stable release
> will be `1.0.0`; until then, keep your backups and expect changes.

## Download

Builds are published on the
[Releases](https://github.com/TaylorHyura/Hyura-Mod-Manager/releases) page.

1. Download the latest release archive.
2. Extract it to a writable folder, such as `C:\Games\HMM`. Do not run it from inside
   the archive or from `Program Files`.
3. Run `HyuraModManager.exe`.

HMM keeps its settings, mods and backups beside the executable, so moving the folder
moves your setup. No installer is required.

If Windows SmartScreen shows a warning for a new build, confirm the download came from
this repository's Releases page before running it.

## Features

- **Game library** - Browse the official and community catalogs, add games, and keep
  covers cached for offline use.
- **Mod installation** - Install one or many `.zip`, `.rar`, `.7z` or `.pak` files.
  Each mod gets its own folder under `InstalledMods`.
- **Engine-aware detection** - Mods are classified as Fluffy, Plugin, Pak, LogicPak,
  Lua, Overwrite or Hyura, and their files are placed where that engine loads them.
- **Hyura packages** - HMM's native format. Archives with a `hyuramod.json` manifest
  install the same way on any engine, with components that mirror the game's folder
  layout. See [Hyura mod format](#hyura-mod-format).
- **Deployment control** - Enable or disable a whole mod, an individual component, or a
  single file. Drag rows to decide which mod wins a file conflict.
- **Backups and restore** - Original game files are moved to `BackupFiles` the first time
  a mod overwrites them, and restored when the mod is disabled or removed.
- **Dependencies** - Per-game loaders such as REFramework or UE4SS, with one-click
  download and installation.
- **Updates and reinstall** - Check installed mods against Nexus and update from a new
  archive without losing the mod's position.
- **Themes and accessibility** - Light, Dark, Graphite Dark and Auto themes, keyboard
  focus styles, and accessible control names.

## Screenshots

<table>
  <tr>
    <td width="50%"><img src="docs/my-games.png" alt="My Games"><br><sub><b>My Games</b> &mdash; your game library</sub></td>
    <td width="50%"><img src="docs/browse.png" alt="Browse"><br><sub><b>Browse</b> &mdash; official and community catalogs</sub></td>
  </tr>
  <tr>
    <td width="50%"><img src="docs/dependencies.png" alt="Dependencies"><br><sub><b>Dependencies</b> &mdash; loaders such as REFramework or UE4SS</sub></td>
    <td width="50%"><img src="docs/settings.png" alt="Settings"><br><sub><b>Settings</b> &mdash; Nexus API key, theme and game paths</sub></td>
  </tr>
</table>

## Requirements

- Windows 10 or 11 (64-bit)
- A writable folder for the app and its data
- Optional: a Nexus Mods API key for metadata and update checks. Manual installation
  works without one.
- If a release note lists the .NET 10 Desktop Runtime as a requirement, install the
  [.NET 10 Desktop Runtime](https://dotnet.microsoft.com/download/dotnet/10.0) before
  launching HMM.

## First run

1. Open **Settings**. Optionally paste a Nexus API key from
   <https://www.nexusmods.com/settings/api-keys>, and choose a theme.
2. On **Home**, switch to **Browse**, add the games you want, then switch back to
   **My Games**.
3. Open a game, choose **Game Settings**, and set the game's install directory.
4. If the game needs one, open **Dependencies** and install its loader, such as
   REFramework or UE4SS.
5. Choose **Install Mod** and select one or more mod archives. HMM extracts, classifies
   and deploys them.
6. Toggle mods on or off, drag rows to set priority, and double-click a mod to manage
   its components or individual files.

The game path is checked during deployment. If the folder does not contain the game
executable, HMM keeps the mod list but does not copy files into it.

## Supported games and engines

Games are data, not code. HMM reads the live catalog from
[HMM-Supported-Games](https://github.com/TaylorHyura/HMM-Supported-Games) and stores the
definition of each game you add under `SupportedGames\<game-id>.json`.

The catalog has two tiers:

- **Officially Supported** - curated and tested by the HMM maintainer.
- **Community Supported** - contributed by the community through pull requests.

Both tiers work in the app. New games can be added to the catalog without an HMM
update. If a game is missing, see the
[catalog contribution guide](https://github.com/TaylorHyura/HMM-Supported-Games/blob/main/CONTRIBUTING.md).

| Format / engine | What HMM understands |
| --- | --- |
| **[Hyura (any engine)](#hyura-mod-format)** | Packages with a `hyuramod.json` manifest whose component folders mirror the game's install layout. |
| **RE Engine** | REFramework plugin DLLs, Fluffy-style `modinfo.ini` packages (including addons), and PAK invalidation for `natives` files. |
| **MT Framework** | `nativePC`-style archives and overwrite mods. |
| **Unreal Engine** | `.pak` mods, LogicMods, loose Lua and DLL mods (UE4SS-style), and overwrite/loader mods. |

## Hyura mod format

Hyura is HMM's native, engine-independent mod format. A Hyura package is a `.zip`,
`.rar` or `.7z` with a `hyuramod.json` manifest, ideally at the archive root. HMM
installs the manifest's components instead of detecting an engine layout, so the same
package works on any engine as long as its component folders mirror the game's install
directory.

```json
{
  "Name": "Example Mod",
  "Description": "Short description shown in HMM.",
  "Version": "1.2.0",
  "Author": "YourName",
  "PreviewImage": "preview.png",
  "Components": [
    { "Name": "Main files", "Type": "Main", "Path": "main" },
    { "Name": "Optional addon", "Type": "Addon", "Path": "addon" }
  ]
}
```

```text
ExampleMod.zip
├─ hyuramod.json
├─ preview.png
├─ main\natives\...      - deployed as-is
└─ addon\natives\...
```

Top-level fields:

| Field | Required | Behaviour |
| --- | --- | --- |
| `Name` | Yes | Mod name shown in HMM and used for the installed folder. |
| `Description` | No | Shown in the details panel. |
| `Version` | No | Defaults to the install date (`dd.MM.yy`); included in the folder name. |
| `Author` | No | Defaults to `Unknown`. |
| `PreviewImage` | No | Path relative to the archive root; copied and resized into the mod's cover. |
| `Components` | Yes | Each entry becomes a component; a manifest with none installs no files. |

Component fields:

| Field | Required | Behaviour |
| --- | --- | --- |
| `Path` | Yes | Folder inside the archive, relative to its root. Entries with an empty path are skipped. |
| `Name` | Recommended | Component label; falls back to the mod name. |
| `Type` | No | `Main` by default. `Addon` (any casing) installs the component disabled. |
| `OrderingPath` | No | Subfolder whose files follow the mod's load order. See below. |
| `OrderingType` | No | How `OrderingPath` is applied. `File` (default) prefixes each file with `<priority>_<id>_`; `Folder` moves the files into the generated folder instead. |
| `Description` | No | Shown in the component manager's details panel. |
| `PreviewImage` | No | Removed from the deployed files; not shown yet. |

What HMM does with the files:

- Everything under `Path` is moved as-is, keeping its folder structure, so it must
  match the game's install directory.
- With `OrderingPath` set, files under it follow the mod's priority: `OrderingType:
  "Folder"` moves them into a generated `<priority>_<id>_<name>_<version>` folder,
  and `File` (the default) prefixes each file with `<priority>_<id>_`.
- Documentation is discarded: files matching `readme`, `read-me`, `read_me`, `credits`,
  `changelog`, `troubleshooting` or `.url`. `.txt` files are kept only when the name
  contains `config`.
- `hyuramod.json` and the declared preview image are not deployed.
- Components that end up with no files are dropped.

HMM searches the whole archive for `hyuramod.json` and uses the first one it finds, so
keep exactly one, at the archive root.

## Backups and safety

Before HMM overwrites a game file for the first time, it moves the original to
`BackupFiles\<game-id>` and records its SHA-256 hash. Disabling or uninstalling the mod
restores the backed-up file once no other enabled mod needs it.

Keep `BackupFiles` until you are happy with your setup. If something goes wrong, disable
the mod in HMM first; manually deleting backups can make the original game files
impossible to restore.

## Data folders

Everything is relative to the folder containing `HyuraModManager.exe`:

| Path | Contents |
| --- | --- |
| `AppSettings.json` | Theme, Nexus API key, added game IDs, and per-game install paths |
| `SupportedGames\` | Catalog entries for the games you added |
| `ModLists\` | Installed mods, components, priorities, and enabled state per game |
| `InstalledMods\` | Extracted mod files, one folder per mod |
| `DownloadedFiles\` | Original mod archives and dependency downloads |
| `BackupFiles\` | Original game files moved aside during deployment |
| `Cache\` | Deploy state, file hashes, and cached cover images |

> The Nexus API key is stored in `AppSettings.json` in plain text. Keep the HMM folder
> private, and do not share or commit `AppSettings.json`.

## Troubleshooting

- **A mod is installed but not visible in the game.** Check the game path in
  **Game Settings**, make sure the mod and its components are enabled, and confirm any
  required loader is installed under **Dependencies**.
- **A game is not in Browse.** Check the catalog first. If it is missing, request or
  add it in [HMM-Supported-Games](https://github.com/TaylorHyura/HMM-Supported-Games).
- **A mod does not appear correctly in HMM.** Check that the archive uses a supported
  format and that its folders match the game's install layout. A Hyura package with a
  `hyuramod.json` manifest is the most reliable option.
- **HMM does not start.** Extract the whole release archive, install the .NET Desktop
  Runtime if the release requires it, and check whether antivirus software quarantined
  a file.
- **I want to undo everything.** Disable or uninstall the mods in HMM so the backups are
  restored. Keep `BackupFiles` until the game is back to normal.

## Reporting issues

Use the [Issues](https://github.com/TaylorHyura/Hyura-Mod-Manager/issues) page for HMM
bugs and feature requests.

Please include:

- the HMM version shown in the sidebar,
- your Windows version,
- the game and mod you were using,
- the exact steps to reproduce the problem,
- screenshots or the full error message when possible.

For game catalog problems, broken download links, or a missing game, use
[HMM-Supported-Games](https://github.com/TaylorHyura/HMM-Supported-Games/issues) instead.
For problems with a specific mod, contact the mod's author.

Do not post API keys, passwords, or personal file paths in an issue.

## Source and contributions

The application source is not public at this time. This repository is used for public
releases and issue tracking.

The best way to contribute right now is to improve game support in
[HMM-Supported-Games](https://github.com/TaylorHyura/HMM-Supported-Games). New catalog
entries and corrections are reviewed before they are merged.

## Third-party

HMM uses [CUE4Parse](https://github.com/Fortnite-API/CUE4Parse) for Unreal Engine package
inspection, [7-Zip](https://www.7-zip.org/) for archive extraction, and the Nexus Mods
API when an API key is provided. It also depends on Newtonsoft.Json,
ini-parser-netstandard and SkiaSharp.

Hyura Mod Manager is not affiliated with Nexus Mods, Capcom, or any game publisher.
Install mods at your own risk, and keep your `BackupFiles` folder until you are happy
with a setup.
