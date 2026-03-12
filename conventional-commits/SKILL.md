---
name: conventional-commits
description: Use when you need to generate, validate, normalize, classify, or explain Git commit messages according to Conventional Commits 1.0.0. Supports commit authoring, SemVer bump inference, breaking-change detection, and footer parsing.
license: MIT
---

# Conventional Commits Skill

Use this skill when a user asks to:

- write a commit message
- rewrite an existing commit message into Conventional Commits format
- validate whether a commit message conforms to the spec
- infer semantic version bumps from commit messages
- explain commit type, scope, body, footer, or breaking-change semantics
- normalize ad hoc commit text into structured commit messages

Do not use this skill when the task is unrelated to Git commit message formatting or release classification.

## Read Order

1. Read [_INDEX.md](./_INDEX.md) for routing and minimal structure.
2. Read [spec.md](./spec.md) for normative syntax, constraints, and parsing rules.
3. Read [faq.md](./faq.md) only when the task involves policy, ambiguity, or workflow edge cases.

## Execution Rules

### [Concept] Generation

- Produce headers in this form:

```text
<type>[optional scope][optional !]: <description>
```

- Use lowercase types unless the repository explicitly follows another convention.
- Keep the description short and action-oriented.
- Add a body only when extra rationale or context is useful.
- Add footers for references, review metadata, or breaking-change details.

### [Concept] Validation

Check the following in order:

1. Header starts with a type.
2. Optional scope is wrapped in parentheses.
3. Optional `!` appears immediately before `:`.
4. `: ` separator exists and is followed by description text.
5. Body begins after one blank line if present.
6. Footer section begins after one blank line if present.
7. Footer tokens use valid trailer-like syntax.
8. `BREAKING CHANGE` is uppercase when used as the special footer token.

### [Concept] SemVer Classification

Apply this precedence:

1. If header contains `!` or footer contains `BREAKING CHANGE:` or `BREAKING-CHANGE:`, classify as `MAJOR`.
2. Else if type is `feat`, classify as `MINOR`.
3. Else if type is `fix`, classify as `PATCH`.
4. Else no implied version bump.

### [Concept] Normalization

- If a change mixes multiple concerns, prefer splitting it into multiple commits.
- If a user gives an invalid or vague message, rewrite it into one clear primary type.
- If the user supplies a custom type, preserve it unless asked to enforce a narrower convention.
- If the user asks for strict spec compliance, reject malformed header separators, malformed scopes, and malformed breaking markers.

## Output Defaults

- For a single commit request, return one final commit message only unless the user asks for explanation.
- For validation, report whether the message is valid and name the first failing rule.
- For rewrite tasks, return the normalized message and keep the meaning unchanged.
- For release inference, return the bump level and the exact triggering rule.

## Constraints

> **[CONSTRAINTS]**
> - Do not invent `BREAKING CHANGE` unless the described change is actually incompatible.
> - Do not infer a SemVer bump from non-standard types unless repository policy says to do so.
> - Do not place `!` anywhere except immediately before `:`.
> - Do not treat `BREAKING CHANGE` as lowercase when strict compatibility is required.

## Reference Files

- [_INDEX.md](./_INDEX.md)
- [spec.md](./spec.md)
- [faq.md](./faq.md)