# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository state

This repository currently contains **no application source code** — it is an [Obsidian](https://obsidian.md/) vault holding project documentation for a coffee store project ("Demo-Project"). There is no package.json, build system, linter, or test suite yet. If source code is added later, this file should be updated with the relevant build/lint/test commands and architecture notes.

## Structure and workflow

All content lives under `docs/`, organized as a pipeline that mirrors the project lifecycle. Each folder has an `index.md` (written in Thai) explaining its purpose and linking to adjacent stages via Obsidian wikilinks (`[[relative/path/index|label]]`). The intended flow is:

```
01-requirements  →  02-design  →  03-testing  →  04-retrospectives
  01-spec              01-prototypes   01-test-plan
  02-plan              02-technical    02-test-result
  03-task
                          ↕
                       05-log (chronological record, referenced throughout)
                       00-archived (superseded/deprecated docs)
```

- **`01-requirements/`** — source of truth for what the system must do: specs (`01-spec`), roadmap/phasing (`02-plan`), and actionable task breakdowns (`03-task`).
- **`02-design/`** — UI/UX prototypes (`01-prototypes`) and technical design: architecture, DB schema, API contracts (`02-technical`). This is the blueprint developers reference once code is written.
- **`03-testing/`** — test plans/cases derived from design (`01-test-plan`) and actual pass/fail results and bugs (`02-test-result`).
- **`04-retrospectives/`** — lessons learned per phase/sprint, informed by test results and the log.
- **`05-log/`** — chronological changelog and decision log; the evidentiary record other sections cite.
- **`00-archived/`** — retired documents. Convention stated explicitly in this folder's index: **do not delete superseded docs, move them here instead** to preserve decision history.

## Conventions to follow

- Docs are written in Thai; match that language when editing or adding to existing `index.md` files.
- Cross-reference related docs using Obsidian wikilink syntax (`[[path/index|Display Text]]`), consistent with the existing files.
- Preserve the numeric prefix ordering (`00-`, `01-`, `02-`, ...) when adding new top-level or nested folders — it encodes the pipeline sequence.
- When a document is deprecated, move it to `00-archived/` rather than deleting it.
