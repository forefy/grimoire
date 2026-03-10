# Implementation Plan

Generated: 2026-03-10

## Priority Queue

Items with remaining work, sorted by priority (highest first).

### 1. agent:gnome
- **Status:** not_implemented
- **Spec detail:** detailed
- **Spec:** `grimoire/agents/gnome.md`
- **Dependencies:** None
- **Why #1:** Only unimplemented agent with a detailed spec. Scribe currently stubs gnome delegation with fallback to direct check creation. Implementing gnome enables proper separation of concerns: scribe decides *what* to build, gnome *builds* it. Unblocks clean autonomous-discovery and finding-discovery flows.
- **Tasks:**
  1. Create `agents/gnome.md` with agent frontmatter (name, description, tools: Read/Grep/Glob/Bash/Write/Edit)
  2. Design system prompt: isolated task executor with plan-based execution, status reporting, comprehensive summary generation
  3. Define task handoff protocol (context passing format, completion criteria)
  4. Update scribe agent to delegate check creation to gnome instead of fallback
  5. Update scribe-distill skill to invoke gnome for check/sigil building
  6. Wire gnome into autonomous-discovery flow (PoC implementation delegation)

### 2. concept:context-building — gadgets implementation
- **Status:** not_implemented (gadgets concept completely absent)
- **Spec detail:** partial
- **Spec:** `grimoire/concepts/context building.md`
- **Dependencies:** None for concept; gadgets integration depends on write-poc, sigil
- **Why #2:** Gadgets are a load-bearing concept for bounty hunting leverage — tricks/patterns that enable critical vulns — yet completely absent from the implementation. Cross-cutting gap affecting write-poc, sigil, and context building.
- **Tasks:**
  1. Document gadget concept and catalog structure (format, frontmatter, indexing)
  2. Add gadget awareness to write-poc workflow (check for applicable gadgets during PoC construction)
  3. Add gadget surfacing to sigil output format ("interesting gadget patterns discovered")
  4. Add gadget collection section to GRIMOIRE.md template in summon
  5. Consider scribe mode for gadget indexing (future)

### 3. skill:semgrep
- **Status:** not_implemented
- **Spec detail:** partial
- **Spec:** `grimoire/skills/sigils/semgrep.md`
- **Dependencies:** None
- **Why #3:** Primary backpressure tool. Unblocks agent:sigil Mode 3 (super-sigil) — semgrep runners spawning validation sigils. Advances concept:backpressure adoption across the plugin.
- **Tasks:**
  1. Create `skills/semgrep/` directory with SKILL.md
  2. Build references covering rule anatomy, testing, and rule merging guidance
  3. Create worked examples of semgrep rules for common vuln patterns
  4. Wire into sigil Mode 3 as super-sigil runner

### 4. concept:trivial-verifiability — falsifiable vs verifiable distinction
- **Status:** partially_adopted
- **Spec detail:** detailed
- **Spec:** `grimoire/concepts/(trivial) verifiability.md`
- **Dependencies:** None
- **Tasks:**
  1. Add verifiability language to write-poc steps 2-3 (goal condition as testable hypothesis)
  2. Add falsifiable vs verifiable distinction to sigil strategy section
  3. Add to familiar: guidance on using falsifiable framing when dismissing
  4. Add to checks: guide toward falsifiable patterns ("find locations where X occurs") not universal negatives

### 5. concept:backpressure — explicit enforcement
- **Status:** adopted (principle), partial (enforcement)
- **Spec detail:** detailed
- **Spec:** `grimoire/concepts/backpressure.md`
- **Dependencies:** skill:semgrep, skill:slither for full enforcement
- **Tasks:**
  1. Create shared reference doc `backpressure-methods.md` listing when to use semgrep/codeql/slither vs agentic checks
  2. Add constraint to agents: "Do not answer 'are all instances of X handled' without a static analysis rule to back it up"
  3. Add to summon: explicit check for backpressure before recommending autonomous findings

### 6. concept:the-original-sin + concept:leverage — explicit in skills
- **Status:** adopted (implicit)
- **Spec detail:** detailed
- **Dependencies:** None
- **Tasks:**
  1. Create shared reference doc `principles.md` covering human judgment, leverage, and the original sin
  2. Update write-poc philosophy section to reference human-directed hypothesis, not autonomous exploitation
  3. Update summon philosophy to emphasize researcher interprets crown jewels, agent only maps

### 7. concept:hypothesis-generation — seeded/unseeded guidance
- **Status:** adopted (core), partial (guidance)
- **Spec detail:** detailed
- **Spec:** `grimoire/concepts/hypothesis generation.md`
- **Dependencies:** None
- **Tasks:**
  1. Frame goal condition as "testable hypothesis" in write-poc
  2. Add seeded vs unseeded guidance to cartography exploration modes
  3. Add to sigil: "If you discover tangential issues, note briefly and suggest spawning a separate sigil"

### 8. flow:finding-discovery — wire scribe→variant loop
- **Status:** partially_adopted
- **Spec detail:** partial
- **Spec:** `grimoire/flows/finding discovery.md`
- **Dependencies:** agent:scribe (done), skill:scribe-distill (done)
- **Tasks:**
  1. Update finding-draft step 6 to suggest variant sigil spawn if pattern is generalizable
  2. Wire scribe-distill output to variant sigil spawn prompt (step 8-9)
  3. Add explicit "run new check against codebase immediately" step to scribe-distill

### 9. flow:autonomous-discovery — complete pipeline
- **Status:** partially_adopted (wired in summon step 9, but loop incomplete)
- **Spec detail:** partial
- **Spec:** `grimoire/flows/autonomous discovery.md`
- **Dependencies:** agent:gnome (#1), agent:sigil (done), agent:familiar (done)
- **Tasks:**
  1. Wire familiar → variant sigil spawn after confirming a finding
  2. Implement gnome delegation for PoC construction
  3. Document full autonomous-discovery cycle as a reference

### 10. skill:slither
- **Status:** not_implemented
- **Spec detail:** partial
- **Spec:** `grimoire/skills/sigils/slither.md`
- **Dependencies:** None
- **Tasks:**
  1. Create `skills/slither/` directory with SKILL.md
  2. Build references covering detector anatomy, documentation requirements, napalm integration
  3. Create worked examples
  4. Wire into sigil Mode 3 as super-sigil runner

### 11. agent:sigil — super-sigil (Mode 3)
- **Status:** partial (Modes 1-2 done)
- **Spec detail:** detailed
- **Spec:** `grimoire/agents/sigil.md`
- **Dependencies:** skill:semgrep (#3), skill:slither (#10)
- **Tasks:**
  1. Implement super-sigil pattern: semgrep/slither runners spawning validation sigils

### 12. infra:personal-grimoire — full structure
- **Status:** partial
- **Spec detail:** partial
- **Spec:** `grimoire/concepts/personal grimoire.md`, `grimoire/agents/scribe.md`
- **Dependencies:** agent:scribe (done)
- **Tasks:**
  1. Define full ~/.grimoire/ directory structure (sigils/, knowledge/, gadgets/)
  2. Implement end-of-audit merge workflow in scribe-distill
  3. Add personal grimoire query to scribe-utilities

### 13. infra:spellbook — user-facing documentation
- **Status:** partial
- **Spec detail:** partial
- **Spec:** `grimoire/agents/scribe.md`
- **Dependencies:** agent:scribe (done)
- **Tasks:**
  1. Add user-facing explanation of spellbook concept to scribe skills or a shared reference

### 14. infra:tomes — format guidance
- **Status:** partial
- **Spec detail:** partial
- **Spec:** `grimoire/skills/summon.md`, `grimoire/ideas/todo.md`
- **Dependencies:** None
- **Tasks:**
  1. Create format guidance doc for tomes
  2. Add "when to create a tome" guidance to summon

### 15. infra:readme-update
- **Status:** stale
- **Spec detail:** n/a
- **Dependencies:** None
- **Tasks:**
  1. Update README.md: Familiar and Scribe are now "Implemented", not "Spec'd"

## Cross-Cutting Adoption

| Concept/Flow | Adoption | Touches | Recommendations |
|---|---|---|---|
| **concept:trivial-verifiability** | partially_adopted | write-poc, familiar, sigil, checks | Add falsifiable-vs-verifiable distinction (Priority Queue #4) |
| **concept:the-original-sin** | adopted | All skills/agents | Add explicit friction-removal language (Priority Queue #6) |
| **concept:backpressure** | adopted (principle) | sigil, checks, write-poc, summon | Implement tool skills + add enforcement (Priority Queue #5) |
| **concept:hypothesis-generation** | adopted | write-poc, cartography, sigil, familiar | Add seeded/unseeded guidance (Priority Queue #7) |
| **concept:context-building** | partially_adopted | summon, cartography, sigil, write-poc | **Gadgets completely missing** (Priority Queue #2) |
| **concept:agent-context** | adopted | All agents | Context hygiene practiced; explicit docs optional |
| **concept:don't-get-in-the-way** | adopted | All skills/agents | Exploration-over-storage implicit; explicit docs optional |
| **concept:personal-grimoire** | partially_adopted | scribe, scribe-distill, summon | Structure incomplete (Priority Queue #12) |
| **flow:autonomous-discovery** | partially_adopted | sigil, familiar, librarian, gnome, scribe | Gnome missing; variant loop not wired (Priority Queue #9) |
| **flow:finding-discovery** | partially_adopted | finding-draft, scribe-distill, sigil | scribe→variant sigil not automatic (Priority Queue #8) |

## Implementation Debt

Skills that work but lack structural depth compared to mature peers (write-poc, checks, cartography pattern: SKILL.md + references/ + examples/ + scripts/).

| Skill | Has | Missing | Priority |
|-------|-----|---------|----------|
| **scribe-distill** | SKILL.md, references/ (2 files) | examples/, scripts/ | Medium |
| **scribe-gc** | SKILL.md | references/, examples/, scripts/ | Low |
| **scribe-utilities** | SKILL.md | references/, examples/, scripts/ | Low |
| **finding-draft** | SKILL.md | references/, examples/, scripts/ | Medium |
| **finding-review** | SKILL.md | references/, examples/, scripts/ | Medium |
| **finding-dedup** | SKILL.md | references/, examples/, scripts/ | Low |

Note: finding-draft/review/dedup load the base `finding` skill (which has full references/examples/scripts), so their own structural gaps are less critical — they inherit supporting material.

## Needs Spec Work

Items with `sketch` spec detail requiring answers before implementation.

### skill:codeql
- No spec file exists at `grimoire/skills/sigils/codeql.md`
- Which languages to support initially?
- What query patterns to focus on?

### skill:triage
- Mentioned in roadmap (`grimoire/ideas/notes.md`)
- What is triage as a skill vs familiar agent triage? Overlap?
- What specific assessment framework?

### skill:finding-utilities
- Brief mention in `grimoire/skills/finding.md`
- What specific utilities beyond index-findings.sh?
- Search, stats, filtering?

### agent:imp
- Spec: brief mention in `grimoire/ideas/notes.md` and `grimoire/ideas/todo.md`
- Manual (user supplies audits) or automated (crawl audit DBs)?
- What external audit sources? (Solodit, Code4rena, OpenZeppelin?)
- How does imp differ from librarian? (imp = adversarial audit analysis, librarian = general reference?)

### concept:gadgets (partial — enough to start, but questions remain)
- How should gadgets be cataloged? Relationship to spellbook?
- What's the indexing/retrieval mechanism during PoC development?
- Can gadgets be expressed as unit tests? What format?

### infra:alchemy
- Mentioned in `grimoire/ideas/notes.md`
- What is "pi" extensibility framework? What are "alloyed agents"?
- Needs full architecture spec before any implementation

### infra:grimoire-guide
- Mentioned in `grimoire/ideas/notes.md`
- Target audience? Format (single guide vs blog series)?
- Content scope (all skills or beginner subset)?

## Completed

Items with status `implemented` and no remaining tasks (0 debt, 0 cross-cutting gaps).

- **skill:finding** — Base knowledge skill. SKILL.md + references + examples + scripts. Full spec coverage.
- **skill:review-cartography** — 6-step verification workflow. Full supporting structure. 0 debt.
- **skill:gc-cartography** — 6-step GC workflow. Full supporting structure. 0 debt.
- **agent:librarian** — External research agent. 2 modes, 6-tier source priority, citation format. 12/16 spec requirements (4 gaps are environment-specific MCP configs).
- **agent:scribe** — Detection module builder. 3 modes, skills suite (distill/gc/utilities). 47/53 spec requirements.
- **agent:familiar** — QA gatekeeper. 3 modes, personality customization. 15/15 spec requirements.
- **infra:audit-directory-structure** — Fully specified in summon SKILL.md step 2.
- **infra:cartography-files** — Format, indexing script, and examples. Complete.
- **infra:finding-files** — Frontmatter schema, filing conventions. Complete.
- **infra:check-files** — Format and design principles documented. Complete.
- **infra:grimoire-file-format** — Living document format. Complete.
- **concept:what-is-grimoire** — Covered by README.md and plugin design.
- **concept:agent-context** — Subagent patterns used throughout all implemented skills/agents.
- **concept:don't-get-in-the-way** — Exploration-first approach in cartography and summon. Minimalism practiced.
