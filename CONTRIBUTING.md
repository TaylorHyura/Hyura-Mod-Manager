# Contributing to Hyura Mod Manager

Thanks for helping HMM. The project is split across three repositories, and each one
accepts a different kind of contribution:

| Repository | What belongs there |
| --- | --- |
| This repository (`Hyura-Mod-Manager`) | Public documentation, release notes, and bug or feature reports |
| [HMM-Supported-Games](https://github.com/TaylorHyura/HMM-Supported-Games) | Game catalog entries, fixes, and cover or link corrections |
| `HyuraModManager` | Application source, private during pre-release; accepts no public code contributions yet |

## Where to contribute

### Game support

Almost all community contributions belong in
[HMM-Supported-Games](https://github.com/TaylorHyura/HMM-Supported-Games). Adding a game
there makes it available in HMM without an app update. Read the
[catalog contribution guide](https://github.com/TaylorHyura/HMM-Supported-Games/blob/main/CONTRIBUTING.md)
for the JSON format, validation command, and review requirements.

### Bugs and feature requests

Open an issue in this repository. Include:

- the HMM version shown in the sidebar,
- your Windows version,
- the game and mod you were using,
- the exact steps to reproduce the problem,
- screenshots or the full error message when possible.

For a game that is missing, has a broken cover, or installs files into the wrong folder,
use the [catalog issue tracker](https://github.com/TaylorHyura/HMM-Supported-Games/issues)
instead. For a problem with a specific mod, contact the mod's author.

### Documentation

Documentation fixes are welcome as issues or pull requests against this repository.
Keep a change focused: one correction or addition per pull request, with a short
imperative title and a body that explains why the change is needed.

## Pull request review

The repository owner reviews every pull request. A change is merged only after it is
approved, unrelated edits are removed, and any required checks pass. The owner may edit
the branch or ask you to update it.

This repository is used for public releases and documentation, so the owner can also
apply small documentation fixes directly for maintenance.

## Security and privacy

- Never include API keys, access tokens, passwords, or personal file paths in an issue,
  pull request, screenshot, or log.
- Do not post links to pirated games, mods, or tools.
- Report malicious or unsafe download links in an issue instead of downloading them.
- If you accidentally expose a secret, say so in the report so it can be removed and
  rotated; deleting it in a later edit is not enough.

## Questions

Open an issue with the `question` label, or comment on an existing pull request.
