# claude-hippocampus
A token-efficient memory architecture for Claude AI projects (cowork). Structured around a lean entry point, a single source of truth for critical data, append-only session logs, selective file loading, and a sleep mode for memory consolidation. No data loss, no duplication.

_**Inspired by how biological memory actually works.**_

<img src="https://github.com/user-attachments/assets/4a744876-9d92-4ce0-a158-e8a9725316e3" width="100" height="100" />

![Version](https://img.shields.io/badge/version-1.0-blue)

## What is this?
The hippocampus is the brain region that transfers short-term memories into long-term storage during sleep. What you experience during the day gets consolidated overnight: redundant details drop away, key facts get filed into stable memory.

This system works the same way. Each session is short-term memory. When you trigger sleep mode, Claude consolidates that session into the right long-term files — routing facts, ticking off actions, archiving what's stale. Nothing is rewritten. Nothing is lost. 

The whole architecture is built to optimise for token savings (selective loading), and long-term memory. 


## Core principles
- One source of truth — critical data lives in exactly one file. No duplication, no drift.
- Append-only — the session log is never edited, only extended. Past entries are archived, never deleted.
- Surgical loading — CLAUDE.md is a map (~50 lines). Only the files a task actually needs get loaded.
- Lossless — data is either active, promoted, or archived. It never disappears.

## Installation
This is a .md file system — no code, no dependencies.
- Copy the file structure into your project
- Adapt file names and sections to your context
- Add all files to your Claude Project context (cowork)
- CLAUDE.md is your entry point — always loaded

## Status
v1.0 — first release. This architecture will evolve. Forks and contributions are welcome.
