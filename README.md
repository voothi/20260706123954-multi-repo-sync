# Multi-Repository Sync and Tag Manager

[![Version](https://img.shields.io/badge/version-v1.0.0-green)](https://github.com/voothi/20260706123954-multi-repo-sync)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A lightweight, powerful Python command-line utility to coordinate repository commits, tagging, and checkouts across multiple directories. This ensures synchronized development workspaces, snapshot version control, and consistent logging of repo states directly from the terminal or subagent tasks.

## Table of Contents
- [Description](#description)
- [Motivation](#motivation)
- [Features](#features)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
  - [Coordinated Repositories](#coordinated-repositories)
  - [Script and Formatting Variables](#script-and-formatting-variables)
- [Usage](#usage)
  - [1. Show Repository Status](#1-show-repository-status)
  - [2. Create Coordinated Tags](#2-create-coordinated-tags)
  - [3. Sequential Repository Commits](#3-sequential-repository-commits)
  - [4. Consolidated Commit and Tag Sync](#4-consolidated-commit-and-tag-sync)
  - [5. Multi-Repo Checkout](#5-multi-repo-checkout)
  - [6. Delete Coordinated Tags](#6-delete-coordinated-tags)
- [Kardenwort Ecosystem](#kardenwort-ecosystem)
- [License](#license)

---

## Description
When working with multiple interrelated repositories (e.g., core libraries, desk interfaces, configurations, and vault assets), maintaining synchronized states is critical. This utility enables users to query git statuses across multiple repositories, sequentially stage and commit dirty workspaces with unique Zettelkasten IDs (ZIDs), apply aligned annotated tags to all repositories in a single run, and checkout specific snapshots or branches simultaneously. The manager supports automated logging in various formats (markdown tables, code blocks, or flat log lines) to record workspace snapshots for history and traceability.

[Return to Top](#multi-repository-sync-and-tag-manager)

## Motivation
Working across multiple independent repositories (e.g., core modules, active user interface clients like [20260629183335-kardenwort-desk](https://github.com/voothi/20260706123954-multi-repo-sync/releases/tag/20260706115523), global desktop hotkey drivers, and personal vault notes) makes it difficult to maintain cross-repository consistency without a monorepo setup. When issues emerge, or when trying to reconstruct a previously working state, manual commits, tags, and Zettelkasten ID (ZID) identifiers can easily become fragmented and out of sync. This is especially true during intensive, late-night coding sessions where local changes are scattered across different folders.

This utility was created to eliminate this complexity. By automating status queries, sequential unique ZID commits, coordinated tags, and workspace checkouts, it guarantees that all repositories are synchronized at distinct slice-in-time snapshots, preventing "history holes" and ensuring perfect traceability.

[Return to Top](#multi-repository-sync-and-tag-manager)


## Features
- **Multi-Repository Status Overview**: Instantly query status, active branch, current commit hash, tags at HEAD, and commit message across all registered repositories in an aligned console grid.
- **Coordinated Tagging**: Create and push annotated git tags with dynamic ZID values to all repositories simultaneously, with dirty worktree safeguards.
- **Sequential Commit with Unique ZIDs**: Stage all changes and commit dirty repositories one by one, introducing tiny pauses to guarantee unique Zettelkasten ID timestamps.
- **Simultaneous Checkouts**: Checkout target branches or tags across all active repositories in one command, with force flags to optionally discard uncommitted work.
- **Flexible Snapshots Logging**: Record release/commit histories in multiple formats: Markdown table, raw flat `.log` file, or detailed Markdown sections with Table of Contents.
- **Working Directory Customization**: Easily target custom workspace scopes or change context directories using the global `-C`/`--cwd` flag.

[Return to Top](#multi-repository-sync-and-tag-manager)

## Project Structure
```text
20260706123954-multi-repo-sync/
├── .gitattributes           # Git attributes configuration
├── .gitignore               # Excludes python caches and local configs
├── 20260706123954-multi-repo-sync.code-workspace # Active multi-repo workspace definition
├── LICENSE                  # MIT License
├── README.md                # Premium documentation
└── multi-repo-sync.py       # Core coordinated sync, tag, and checkout script
```

[Return to Top](#multi-repository-sync-and-tag-manager)

## Configuration

Configuring the utility is managed directly inside the python script `multi-repo-sync.py` by modifying the global variables at the top of the file.

### Coordinated Repositories
The `REPOS` dictionary defines the local absolute paths to the git repositories managed by the utility:
```python
REPOS = {
    "desk": r"U:\voothi\20260629183335-kardenwort-desk",
    "autohotkey": r"U:\voothi\20240411110510-autohotkey",
    "core": r"U:\voothi\20241223170748-kardenwort",
    "goldendict": r"U:\voothi\20260113230706-goldendict",
    "vault": r"U:\voothi.vault"
}
```

### Script and Formatting Variables
- **`ZID_SCRIPT`**: Absolute path to the central Zettelkasten ID generator script.
- **`DEFAULT_LOG_FILENAME`**: Output filename for markdown-based history logs (defaults to `multi-repo-sync.md`).
- **`GIT_REMOTE`**: Remote repository name used for push actions (defaults to `origin`).
- **`LOG_COMMIT_VAL`**: Information to record for repository updates (`"hash"`, `"msg"`, or `"both"`).
- **`LOG_FORMAT`**: Layout style of markdown history files (`"table"`, `"code"`, or `"log"`).
- **`DEFAULT_CWD`**: Default context folder where commands are executed.

[Return to Top](#multi-repository-sync-and-tag-manager)

## Usage

The utility is executed as a command-line tool with various subcommands to handle different repository lifecycle stages.

### 1. Show Repository Status
View the branch status, latest commit, applied tags, and commit messages across all coordinated directories:
```powershell
python multi-repo-sync.py status
```

### 2. Create Coordinated Tags
Apply an annotated git tag with the current ZID (or custom tag name) across all repositories. Use `-p`/`--push` to push tags to the remote origin, and `-l`/`--log-file` to record a snapshot of the repo states:
```powershell
python multi-repo-sync.py tag -p -l U:\voothi.vault\multi-repo-sync.md
```

### 3. Sequential Repository Commits
Automatically detect dirty repositories, stage all changes, and commit them sequentially with unique ZIDs. The tool pauses 1.1 seconds between repositories to ensure each gets a unique ZID:
```powershell
python multi-repo-sync.py commit -m "{zid} to desk" -l U:\voothi.vault\multi-repo-sync.md
```

### 4. Consolidated Commit and Tag Sync
Run the sequential commit phase, followed by a coordinated tagging phase across all repos. This provides a single-step way to snapshot local progress:
```powershell
python multi-repo-sync.py sync -p -l U:\voothi.vault\multi-repo-sync.md
```

### 5. Multi-Repo Checkout
Simultaneously checkout a specific branch, commit hash, or tag name across all coordinated repos:
```powershell
python multi-repo-sync.py checkout "20260706124714-snapshot-desk"
```

### 6. Delete Coordinated Tags
Remove a specific tag name from all active repository paths:
```powershell
python multi-repo-sync.py delete "20260706124714-snapshot-desk"
```

[Return to Top](#multi-repository-sync-and-tag-manager)

## Kardenwort Ecosystem

This project is part of the **[Kardenwort](https://github.com/kardenwort)** environment, designed to create a focused and efficient learning ecosystem.

[Return to Top](#multi-repository-sync-and-tag-manager)

## License
MIT License. See LICENSE file for details.

[Return to Top](#multi-repository-sync-and-tag-manager)
