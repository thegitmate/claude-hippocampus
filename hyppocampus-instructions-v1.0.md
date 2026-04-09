# Optimised Context System — Setup & Operating Instructions
*Generic template. Adapt file names and sections to your project.*

---

## Philosophy

Claude's context window is shared across everything: files, messages, tool outputs. Every token loaded costs. The goal is **surgical loading** — load only what the current task needs, while keeping all data intact and retrievable.

Three principles:
1. **One source of truth** — critical data lives in exactly one place. Duplication causes drift and errors.
2. **Append-only memory** — never rewrite past log entries. Append to the top. Archive old entries rather than deleting.
3. **Lossless** — no data is ever deleted. It either stays in active files, gets promoted to stable files, or gets archived. Nothing disappears.

---

## File Architecture

```
CLAUDE.md                        ← entry point, always loaded, max ~50 lines
context/
  quick-ref.md                   ← critical static data (addresses, IBANs, contacts)
  memory.md                      ← append-only session log
  sleep-mode.md                  ← memory consolidation protocol
  projects.md                    ← active project status + pending actions
  [client].md                    ← client profile, preferences, contract
  [services].md                  ← scope, pricing, payment terms
  [domain-knowledge].md          ← topic-specific reference (e.g. sourcing, logistics)
  archive/
    memory-YYYY-MM.md            ← old memory entries (append-only, never auto-loaded)
    [completed-project].md       ← closed projects
```

---

## CLAUDE.md — Structure (max ~50 lines)

```markdown
# [Project Name] — Claude Context

## AI Rules
[tone, language, formatting rules]

## Business Identity
[1–2 lines: what the business does, legal entity, key identifier]
→ Critical static data: `context/quick-ref.md`

## Active Projects
| Project | Phase | Next action |
|---|---|---|
| [Project A] | [Phase] | [Single next step] |

## Selective Loading Protocol
*Always read quick-ref.md for any task involving addresses, contacts, or bank details.*
| Task type | Read |
|---|---|
| Quick task | quick-ref.md |
| Client comms | quick-ref.md + [client].md |
| Active project work | quick-ref.md + projects.md |
| Invoicing / finance | quick-ref.md + [services].md |
| Domain research | [domain-knowledge].md |
| Full context | all files |

## Context Files
| File | Contains |
|---|---|
| `context/quick-ref.md` | ⚡ Addresses, bank details, key contacts |
| `context/memory.md` | Session log — append-only |
| `context/sleep-mode.md` | **"Sleep mode"** = memory consolidation. When user says "sleep mode", read and execute. |
| [other files] | [description] |

## Update Protocol
After each session: append to top of `context/memory.md`.
Monthly: promote stable facts to relevant files. Archive entries >60 days.
```

**Rules:** No contact details, no prices, no project detail in CLAUDE.md — those live in their own files. CLAUDE.md is a map, not a repository.

---

## quick-ref.md — Single Source of Truth

Contains all critical static data that, if wrong, causes downstream errors:
- Business: legal name, address, registration numbers, bank details, email
- Client(s): delivery address, key contacts + roles + emails
- Active suppliers / partners: name, contact, email, current status

**Rule:** Edit only when a fact is verified directly. Never copy this data into other files — reference this file instead.

---

## memory.md — Append-Only Session Log

```markdown
# Memory — Running Log
*Append-only. New entries go at TOP, below this header, above previous entries.*
*Never edit or rewrite past entries.*
*Archive rule: entries >60 days old → append to context/archive/memory-YYYY-MM.md, delete from here.*

---

<!-- APPEND NEW ENTRIES BELOW THIS LINE -->

## YYYY-MM-DD — [Topic]
- [decision / status change / new fact] — **bold key data**
- Only what isn't already captured in other files
```

**What belongs here:** decisions with lasting impact, status changes, new facts not yet in any file, document references, confirmed numbers.
**What doesn't:** reasoning that led to a conclusion already stated, duplicates of what's in project files, drafts not sent, passed scheduling.

---

## Sleep Mode — Memory Consolidation

When user says **"sleep mode"**, Claude reads `context/sleep-mode.md` and executes it. This is not rest — it is active memory consolidation (like human sleep: short-term → long-term).

### sleep-mode.md must contain:

**Step 1 — Triage:** Classify every piece of new session knowledge:
- Keep → route to the right file
- Discard → ephemeral, already captured elsewhere, or not worth long-term storage

**Step 2 — Routing table:**
| What | Op | File |
|---|---|---|
| Static data changed | EDIT specific field | `quick-ref.md` |
| Pending action completed | EDIT `[ ]` → `[x]` | `projects.md` |
| New pending action | EDIT: append to Pending Actions | `projects.md` |
| Project phase changed | EDIT project table row | `CLAUDE.md` + `projects.md` |
| Session events / decisions | APPEND entry to top | `memory.md` |
| Stable fact (used 3+ sessions) | ADD to relevant file, note promoted | `[client].md` / `[services].md` / etc. |

**Step 3 — Write memory.md entry** (append only, never edit past entries)

**Step 4 — Archive check** (if memory.md > ~150 lines or monthly):
1. APPEND old entries to `context/archive/memory-YYYY-MM.md`
2. DELETE from memory.md
3. Note in new memory entry: *"Entries pre-[date] archived to archive/memory-YYYY-MM.md"*

### Lossless guarantee
- Nothing is deleted without first being appended to an archive file
- Archive files are never auto-loaded but always retrievable on request
- quick-ref.md is never cleared — only updated
- projects.md uses checkbox ticks, not deletions — completed actions remain visible until phase closes

---

## Operating Rules (summary)

| Rule | Detail |
|---|---|
| memory.md | Append only. Never edit past entries. |
| quick-ref.md | Single source of truth. Edit only on verified change. |
| projects.md | Surgical edits: tick checkboxes, append actions. No full rewrites. |
| Archive files | Append only. Never rewrite. Never auto-loaded. |
| CLAUDE.md | Map only. No duplicated content from other files. |
| Sleep mode | Lossless consolidation — route, don't rewrite. |
