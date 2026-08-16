# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository state

This repository currently contains **no application source code** — it is an [Obsidian](https://obsidian.md/) vault holding project documentation for a coffee store project ("Demo-Project"). There is no package.json, build system, linter, or test suite yet. If source code is added later, this file should be updated with the relevant build/lint/test commands and architecture notes.

## Project context

**โจทย์ 1:** ผู้ประกอบการต้องการเปิดร้านกาแฟ และต้องการระบบช่วยรับ Order จากลูกค้า โดยให้ลูกค้าสามารถสั่ง Order ได้เองจากที่โต๊ะ (self-order) แทนการเรียกพนักงานมารับออเดอร์

This is the driving business problem for the current phase of work — every requirement/backlog item should trace back to enabling a customer to self-order from their table.

## Current focus: Requirements & Product Backlog

Work right now is concentrated in **`docs/01-requirements/`**. `03-testing/` and `04-retrospectives/` are downstream stages and still just skeleton `index.md` files. `02-design/` is further along: `02-design/01-prototypes/user-journey-self-order.md` (see `feature-list.md` and the second automation pipeline below) is generated alongside the feature list to keep requirement traceability visible early, and `02-design/02-technical/design-system.md` holds the product's design system (Brand Identity & CI, Design Tokens, UI Components & Patterns, UX Guidelines).

- **`01-spec/`** — feature requirements and user stories written against โจทย์ 1 above (e.g. "ในฐานะลูกค้า ฉันต้องการสแกน QR ที่โต๊ะเพื่อดูเมนูและสั่งอาหาร"), plus explicit in-scope/out-of-scope boundaries for this phase. File naming convention: `{YYYYMMDD}-{2-digit running no}-{english-kebab-case-topic}.md`, running number resets per day.
- **`02-plan/`** — how spec items are phased/sequenced (roadmap, milestones).
- **`03-task/`** — granular task breakdown for an in-progress spec item (to-do list, assignee, status), used once an item moves from "backlog" to "being worked on."
- **`backlog.md`** (at the `01-requirements/` root, not a numbered subfolder) — the **Product Backlog**: a single prioritized table listing every spec doc, its priority, and its status. This is the canonical backlog artifact. When asked to write or update the Product Backlog, update this file, not `03-task/`.
- **`feature-list.md`** (at the `01-requirements/` root) — a **MoSCoW-prioritized feature list** derived from `backlog.md` + `01-spec/`: one row per feature (a single spec doc can split into several features), each with a summary table entry plus a detailed section carrying the MoSCoW rating and the reasoning behind it. Kept in sync by the automation pipeline below — don't hand-edit MoSCoW ratings without also reconciling the source spec/backlog reasoning.

Every new spec or backlog entry should be traceable back to the โจทย์ above; if a proposed item doesn't serve "ลูกค้าสั่ง Order ได้เองจากที่โต๊ะ", flag it rather than assuming it belongs in this phase.

### Automation: requirement → backlog pipeline

Turning a raw requirement into documentation is handled by a skill + subagent pair rather than done ad hoc:

- **Skill** [`requirement-to-backlog`](.claude/skills/requirement-to-backlog/SKILL.md) — the interactive front door. Takes a raw requirement from the user, checks `01-spec/` for overlapping existing docs, and asks clarifying questions via `AskUserQuestion` whenever anything is ambiguous — every such question must offer **at least 3 concrete options**, not just yes/no.
- **Subagent** [`requirement-writer`](.claude/agents/requirement-writer.md) — invoked by the skill only after clarification is done. Never asks the user anything; writes the spec doc under `01-spec/` (or amends an existing one), updates `backlog.md`, and appends a summary to `05-log/{YYYYMMDD}-log.md`.

Prefer invoking the `requirement-to-backlog` skill over manually drafting spec docs, so the backlog and log stay in sync automatically.

### Automation: backlog → feature list + user journey pipeline

A second skill + subagent pair keeps the feature list and user journey in sync with the backlog, rather than drafting them ad hoc:

- **Skill** [`backlog-to-feature-journey`](.claude/skills/backlog-to-feature-journey/SKILL.md) — the interactive front door. Audits `backlog.md`/`01-spec/` for drift (new specs missing from the feature list, features whose source spec was archived, journey steps that no longer match a spec), and asks clarifying questions via `AskUserQuestion` (same **at least 3 concrete options** rule as above) only when something is genuinely ambiguous — otherwise it proceeds with a documented default.
- **Subagent** [`feature-list-writer`](.claude/agents/feature-list-writer.md) — writes/updates `01-requirements/feature-list.md`. Never asks the user anything.
- **Subagent** [`user-journey-writer`](.claude/agents/user-journey-writer.md) — writes/updates `02-design/01-prototypes/user-journey-self-order.md` as a Mermaid `flowchart TD` with a step-by-step explanation mapped back to requirement docs. Never asks the user anything. **Hard rule: must never draw a "call staff to take the order" step** in any journey — self-order exists specifically to replace that, per โจทย์ 1.

The two subagents run in parallel and only write their own artifact file; the skill itself writes the `05-log/{YYYYMMDD}-log.md` entry afterward (avoids both subagents racing to edit the same log file). Prefer invoking `backlog-to-feature-journey` over manually editing `feature-list.md` or the user-journey doc.

## Structure and workflow

All content lives under `docs/`, organized as a pipeline that mirrors the project lifecycle. Each folder has an `index.md` (written in Thai) explaining its purpose and linking to adjacent stages via Obsidian wikilinks (`[[relative/path/index|label]]`). The intended flow is:

```
01-requirements  →  02-design  →  03-testing  →  04-retrospectives
  01-spec              01-prototypes   01-test-plan
  02-plan              02-technical    02-test-result
  03-task
  backlog.md
                          ↕
                       05-log (dated files: {YYYYMMDD}-log.md)
                       00-archived (superseded/deprecated docs)
```

- **`01-requirements/`** — source of truth for what the system must do: specs (`01-spec`), roadmap/phasing (`02-plan`), task breakdowns (`03-task`), and the master `backlog.md`.
- **`02-design/`** — UI/UX prototypes (`01-prototypes`) and technical design: architecture, DB schema, API contracts, design system (`02-technical`). This is the blueprint developers reference once code is written. Populated so far: `01-prototypes/user-journey-self-order.md` (see the automation pipeline above) and `02-technical/design-system.md` (Brand Identity & CI, Design Tokens, UI Components & Patterns, UX Guidelines) — architecture, DB schema, and API contracts are still pending.
- **`03-testing/`** — test plans/cases derived from design (`01-test-plan`) and actual pass/fail results and bugs (`02-test-result`).
- **`04-retrospectives/`** — lessons learned per phase/sprint, informed by test results and the log.
- **`05-log/`** — chronological changelog and decision log, one file per day (`{YYYYMMDD}-log.md`); the evidentiary record other sections cite.
- **`00-archived/`** — retired documents. Convention stated explicitly in this folder's index: **do not delete superseded docs, move them here instead** to preserve decision history.

## Conventions to follow

- Docs are written in Thai; match that language when editing or adding to existing `index.md` files.
- Cross-reference related docs using Obsidian wikilink syntax (`[[path/index|Display Text]]`), consistent with the existing files.
- Preserve the numeric prefix ordering (`00-`, `01-`, `02-`, ...) when adding new top-level or nested folders — it encodes the pipeline sequence.
- When a document is deprecated, move it to `00-archived/` rather than deleting it.
