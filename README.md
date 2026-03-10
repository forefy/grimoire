<p align="center">
  <img src="grimoire_logo.png" alt="Grimoire" width="300" />
</p>

<h1 align="center">Grimoire</h1>

<p align="center">
  <strong>A security research toolkit that learns.</strong><br/>
  Claude Code plugin that turns vulnerability discovery into structured, reproducible proof — then builds detection modules so the same class of bug is found automatically next time.
</p>

<p align="center">
  <a href="#installation"><img src="https://img.shields.io/badge/Claude_Code-Plugin-8B5CF6?style=flat-square" alt="Claude Code Plugin" /></a>
  <img src="https://img.shields.io/badge/version-0.1.0-blue?style=flat-square" alt="Version 0.1.0" />
  <img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" alt="License MIT" />
  <img src="https://img.shields.io/badge/vuln_classes-20%2B-critical?style=flat-square&color=dc2626" alt="20+ vulnerability classes" />
</p>

---

## Why Grimoire?

Security researchers live in the gap between *finding* a bug and *proving* it exists. That gap eats hours — writing PoCs, structuring findings, re-investigating the same code flows, running the same manual checks on every new engagement.

Grimoire closes the loop. You find a vulnerability, Grimoire helps you prove it, document it, and then **learn from it** — building detection modules that automatically hunt for the same class of bug in every future audit.

It's a [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin. No build step, no runtime, no dependencies. Install it and its skills activate inside Claude.

### Philosophy

Grimoire is built on a few hard convictions from real-world security research:

- **Leverage, not automation.** Grimoire makes expert researchers faster. It doesn't try to replace the human — it removes friction so you can think harder about what matters.
- **Trivial verifiability.** Every claim an agent makes should be checkable in seconds. Grimoire asks questions that produce concrete, verifiable artifacts — not hand-wavy summaries.
- **Backpressure.** Never ask an agent a completeness question without a way to verify the answer. Grimoire pairs agentic analysis with static analysis tools (Semgrep, Slither, CodeQL) that provide ground truth.
- **Fresh context over stale docs.** Agents re-explore and rediscover. Grimoire stores *pointers* to code (cartography), not *summaries* of code, so context is always current.

## How It Works

Grimoire organizes security research into a pipeline of **skills** (structured workflows you invoke) and **agents** (autonomous specialists that work in the background).

```
summon           Initialize workspace, build context, spawn detection swarm
    ↓
cartography      Map code flows into reusable navigation files
    ↓
checks           Run agentic pattern detection across the codebase
    ↓
write-poc        Produce annotated, executable proof-of-concept exploits
    ↓
finding          Document vulnerabilities with structured best practices
    ↓
scribe           Learn from findings → build detection modules → hunt variants
    ↓
  ╰──→ next audit (sigils carry forward)
```

Each finding you confirm can become a reusable **sigil** — a detection module that runs automatically at the start of your next engagement. The system gets better the more you use it.

## Skills

### `summon` — Initialize a Grimoire workspace

Sets up audit infrastructure on a new codebase. Builds initial context by answering: *What does this project do? What's the architecture? What are the crown jewels?* Produces `GRIMOIRE.md` — a living contextual map loaded into every agent context — then spawns a swarm of detection checks against the codebase.

```
grimoire/
├── findings/         # Your findings
├── sigil-findings/   # Automated findings from detection modules
├── spells/           # Project-specific detection modules
├── cartography/      # Code flow maps
├── tomes/            # Deep-dive notes
└── tmp/
GRIMOIRE.md           # Living context map (~150 lines, cross-linked)
```

### `cartography` — Map code flows

Explores a code flow and documents *where to look* — entry points, key components, flow sequence, security-relevant notes. Cartography files are lightweight pointers, not code summaries, so they stay accurate as the codebase changes. Build context once, reload it in seconds.

Companion skills **`review-cartography`** (verify and refine) and **`gc-cartography`** (merge overlapping flows) keep the map clean.

### `checks` — Agentic vulnerability detection

Simple markdown files that describe one vulnerability pattern each. Grimoire spawns a subagent per check with isolated context — the check description, the codebase, and nothing else. Many focused checks beat one complex scanner.

```markdown
---
name: debug-assertions-in-security-paths
severity-default: medium
confidence: medium
tools: [Grep, Read, Glob]
---

## Pattern
Search for debug_assert!() in authentication, authorization, and validation paths...
```

### `write-poc` — Proof of Concept generation

The core incantation. Six-phase workflow that produces annotated, executable PoCs:

1. **Gather** — vulnerability class, root cause, attack surface, impact
2. **Approach** — minimum viable proof strategy
3. **Write** — annotated code with benign payloads
4. **Format** — adapt to vuln type (script, curl, Foundry fork test, etc.)
5. **Context** — reproduction steps, impact analysis, remediation
6. **Review** — verify safety, completeness, clarity

Covers 20+ vulnerability classes across web, systems, cryptography, and smart contracts:

<table>
<tr>
<td width="33%" valign="top">

**Web**
- SQL Injection
- XSS
- SSRF
- Auth Bypass
- IDOR
- Race Conditions
- Deserialization

</td>
<td width="33%" valign="top">

**Systems & Crypto**
- Buffer Overflow
- Format Strings
- Use-After-Free
- Integer Overflow
- Weak Randomness
- Supply Chain

</td>
<td width="33%" valign="top">

**Smart Contracts**
- Reentrancy
- Price Oracle Manipulation
- Flash Loan Attacks
- ERC4626 Vault Issues
- Rounding Errors
- Access Control

</td>
</tr>
</table>

### `finding` — Structured vulnerability documentation

Finding files follow a strict format: **where** (component) + **how** (mechanism) + **what** (impact) in the title, mandatory Description and Recommendation sections, optional Details and PoC reference. Three companion skills handle the lifecycle:

- **`finding-draft`** — construct a new finding from observations
- **`finding-review`** — fact-check, harden, and verify against best practices
- **`finding-dedup`** — identify and resolve duplicate/overlapping findings

## Agents

Grimoire's agents are autonomous specialists that handle background work:

| Agent | Role | Status |
|-------|------|--------|
| **Sigil** | Single-context vulnerability hunter. Runs checks and static analysis, reports findings for triage. | Implemented |
| **Librarian** | External research. Answers directed questions about specs, docs, and prior art with references. | Implemented |
| **Familiar** | QA gatekeeper. Triages all agent findings before they reach you. Skeptical by default. | Spec'd |
| **Scribe** | Learning engine. Turns confirmed findings into reusable detection modules (sigils). | Spec'd |
| **Gnome** | Isolated task executor. Implements PoCs and detection modules with focused context. | Spec'd |

### The learning loop

```
You confirm a finding
    ↓
Scribe analyzes: can this be detected automatically?
    ↓
Gnome builds a sigil (Semgrep rule / Slither module / agentic check)
    ↓
Sigil runs immediately → variant analysis on current codebase
    ↓
Sigil stored in personal grimoire (~/.grimoire/)
    ↓
Next audit: summon loads your sigils → automatic detection from day one
```

Every audit makes the next one better.

## Installation

```bash
claude --plugin-dir /path/to/grimoire
```

No dependencies. No build step. No configuration. Skills and agents auto-discover via the plugin manifest.

## Usage

Start any engagement with `summon`:

```
You:      "Summon grimoire on this codebase"
Grimoire: → Analyzes project structure, architecture, integrations
          → Identifies crown jewels and attack surface
          → Writes GRIMOIRE.md contextual map
          → Spawns detection checks across the codebase
          → Surfaces initial findings for triage
```

Then work naturally — Grimoire's skills trigger from context:

```
"Map the authentication flow"              → cartography
"Write a PoC for the reentrancy I found"   → write-poc
"Document this as a finding"               → finding-draft
"Review my findings before submission"     → finding-review
"Check for duplicates"                     → finding-dedup
```

## Safety

Grimoire enforces strict safety invariants across all skills:

- **Benign payloads only** — `alert(1)`, `sleep()`, `id` — never destructive commands
- **Parameterized targets** — `localhost`, `$TARGET` — never hardcoded production URLs
- **Minimum viable proof** — demonstrate the issue exists, nothing beyond
- **Authorized contexts only** — pentesting, bug bounty, coordinated disclosure, CTF

## Project Structure

```
grimoire/                     # Human-written specs (read-only, source of truth)
├── agents/                   # Agent specifications
├── skills/                   # Skill specifications
├── concepts/                 # Design philosophy
├── flows/                    # Multi-step workflow specs
└── ideas/                    # Roadmap and research notes

skills/                       # Implemented skills (where development happens)
├── write-poc/                # PoC generation (references, examples)
├── summon/                   # Workspace initialization
├── cartography/              # Code flow mapping
├── review-cartography/       # Flow verification
├── gc-cartography/           # Flow deduplication
├── checks/                   # Agentic pattern detection
├── finding/                  # Finding format & best practices
├── finding-draft/            # Finding construction
├── finding-review/           # Finding verification
└── finding-dedup/            # Finding deduplication

agents/                       # Implemented agents
├── sigil.md                  # Autonomous vulnerability hunter
└── librarian.md              # External research specialist

.claude-plugin/plugin.json    # Plugin manifest (auto-discovers everything)
```

Specs and implementation are strictly separated. The `grimoire/` directory is the source of truth — never modified during development. Skills in `skills/` implement those specs.

## Contributing

Grimoire grows with the knowledge inscribed in it:

- **Vulnerability patterns** — Add checks to `skills/checks/examples/`
- **PoC references** — Expand coverage in `skills/write-poc/references/`
- **New skills** — Create new skill directories in `skills/` (auto-discovered)
- **Agent specs** — Propose new agents in `grimoire/agents/`

## License

MIT

---

<p align="center">
  <sub>Built for those who find the flaws so others can fix them.</sub>
</p>
