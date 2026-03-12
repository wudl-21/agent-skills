# Copilot Agent Skills

🌐 **English** | [中文](README_zh.md)

A collection of VS Code GitHub Copilot Agent Skills for documentation processing, commit workflow standardization, and game modding.

Agent Skills follow the [open Agent Skills standard](https://agentskills.io) and are automatically discovered by GitHub Copilot in VS Code. Each skill is a folder containing a `SKILL.md` file that tells Copilot what the skill does and how to use it.

This repository is authored against the open Agent Skills format and tested for GitHub Copilot in VS Code.

## Skills at a Glance

| Skill | Slash Command | Description |
|---|---|---|
| [`conventional-commits`](#conventional-commits) | `/conventional-commits` | Generate, validate, normalize, classify, and explain Git commit messages using the Conventional Commits 1.0.0 specification |
| [`knowledge-optimizer`](#knowledge-optimizer) | `/knowledge-optimizer` | Convert raw documentation (HTML, XML, verbose Markdown) into high-density, structured Markdown optimized for AI knowledge bases |
| [`teardown-scripting`](#teardown-scripting) | `/teardown-scripting` | Generate, debug, and modify Lua 5.1 scripts for the Teardown game engine (Singleplayer & Multiplayer) |

## Installation

Choose one of the following methods:

### Personal (recommended)

Clone this repository into the standard personal skills directory. Copilot will discover all skills automatically.

**Windows**
```powershell
git clone https://github.com/wudl-21/agent-skills.git "$env:USERPROFILE\.copilot\skills"
```

**macOS / Linux**
```bash
git clone https://github.com/wudl-21/agent-skills.git ~/.copilot/skills
```

> If you already have files in that directory, clone into a subdirectory and copy the individual skill folders instead.

### Per-project

Copy the skill folder(s) you need into your project's `.github/skills/` directory:

```bash
cp -r conventional-commits /path/to/your-project/.github/skills/
cp -r knowledge-optimizer /path/to/your-project/.github/skills/
```

### Custom path

If you store skills in a non-standard location, add the path to your VS Code settings:

```json
// .vscode/settings.json or User Settings
{
  "chat.agentSkillsLocations": ["/your/custom/path/to/skills"]
}
```

## Usage

### Auto-invocation

Describe your task in natural language in Copilot Chat. Skills-compatible clients use each skill's `description` field to decide when to load it, so clear task wording improves matching.

> *"Convert this HTML API reference into a clean knowledge-base Markdown file."*  
> *"Rewrite this commit message into Conventional Commits format and tell me whether it is a breaking change."*  
> *"Write a Teardown multiplayer mod that gives players a speed boost on kill."*

### Manual invocation

Type the slash command in Copilot Chat to invoke a skill explicitly:

```
/conventional-commits
/knowledge-optimizer
/teardown-scripting
```

---

## Skill Details

### conventional-commits

**Purpose:** Generate, validate, normalize, classify, and explain Git commit messages using the Conventional Commits 1.0.0 specification.

**Architecture**
- Keeps the main `SKILL.md` focused on routing and execution rules
- Uses a small `_INDEX.md` as the entry point for structure and task routing
- Splits detailed material into:
  - `spec.md` for normative syntax, parsing, validation, and SemVer rules
  - `faq.md` for policy decisions, ambiguity, and workflow edge cases

**What it supports**
- Commit authoring and rewrites into canonical format: `<type>[optional scope][optional !]: <description>`
- Validation of header, body, footer, and breaking-change markers
- SemVer bump inference:
  - `MAJOR` for `!` or `BREAKING CHANGE`
  - `MINOR` for `feat`
  - `PATCH` for `fix`
- Explanation of commit components, footer parsing, and common invalid patterns

**Example prompts**
```
/conventional-commits Rewrite this as a Conventional Commit: updated auth and removed the legacy login path
```
```
/conventional-commits Is `feat(api)!: remove v1 endpoints` valid, and what version bump does it imply?
```

---

### knowledge-optimizer

**Purpose:** Transform messy, human-readable documentation into a high-information-density local knowledge base optimized for LLM and Agent consumption.

**Accepted input formats**
- Raw HTML / XML (paste directly or provide file contents)
- PDF-extracted text
- Verbose or poorly-structured Markdown

**What it produces**
- De-noised Markdown with all HTML/XML tags, ads, and filler text removed
- Standardized heading conventions for easy Agent-based searching:
  - `### [API] FunctionName(args)` for functions/methods
  - `### [Event] EventName(args)` for events/callbacks
  - `### [Class] ClassName` for classes/modules
  - `### [Concept] ConceptName` for core concepts
- Concise bullet points in place of long paragraphs
- Code blocks with inline comments for context
- `> **[CONSTRAINTS]**` blocks highlighting pitfalls and version restrictions
- For large documents: multiple split files + an `_INDEX.md` navigation file

**Example prompts**
```
/knowledge-optimizer Here is the raw HTML from the Stripe API reference. Convert it into a structured knowledge base.
```
```
/knowledge-optimizer Clean up this verbose Markdown documentation and optimize it for use as an Agent knowledge base.
```

---

### teardown-scripting

**Purpose:** Generate, debug, and modify Lua 5.1 scripts for the [Teardown](https://teardowngame.com) game engine, covering both Singleplayer and Multiplayer mods.

**Key behavior**
- Always determines target mode first — **Singleplayer (`sp`)** and **Multiplayer (`mp`)** APIs are mutually incompatible. If you don't specify, the skill will ask.
- Starts from the bundled top-level index and follows the relevant references under `docs/` as needed:
  - **SP:** 23 API section files + scripting tips
  - **MP:** 25 API section files + mplib module library + scripting tips
- Enforces Lua 5.1 rules (1-based indexing, `--` comments, no bitwise operators)

**Multiplayer-specific rules enforced automatically**
- `#version 2` at the top of every MP script
- Logic separated into `server.*` and `client.*` tables
- No `update(dt)` callback (MP only has `tick(dt)`)
- mplib modules (hud, stats, teams, spawn, etc.) used where applicable

**Example prompts**
```
/teardown-scripting Singleplayer mod: paint all shapes within 5 meters of the player red when I press E.
```
```
/teardown-scripting Multiplayer mod: track kill counts per player and display a leaderboard on screen.
```

---

## Compatibility and Validation

- Target format: open Agent Skills standard
- Tested host: GitHub Copilot in VS Code
- External runtime requirements: none for skill loading; `teardown-scripting` still depends on Teardown's actual runtime and mod format when you execute generated code

Validate individual skills with the reference validator:

```bash
skills-ref validate ./conventional-commits
skills-ref validate ./knowledge-optimizer
skills-ref validate ./teardown-scripting
```

## Scope and Limits

- `conventional-commits` follows the Conventional Commits 1.0.0 spec and generic best practices; repository-specific commit policies may still override default type lists, scope rules, or release semantics.
- `knowledge-optimizer` restructures documentation you provide or expose in-context; it does not make undocumented claims authoritative.
- `teardown-scripting` relies on the bundled Markdown knowledge base in this repository. If Teardown updates its APIs or multiplayer behavior, generated code should be checked against current game/runtime behavior.

---

## License

MIT — see [LICENSE](LICENSE) for details.
