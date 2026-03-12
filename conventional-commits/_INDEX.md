# Conventional Commits Knowledge Base

- Source: Conventional Commits 1.0.0 HTML page in the workspace.
- Purpose: compact agent-facing reference for parsing, validating, and generating commit messages.
- Canonical header grammar: `<type>[optional scope][optional !]: <description>`

## Files

- [spec.md](./spec.md): normative rules, grammar, SemVer mapping, examples, parser constraints.
- [faq.md](./faq.md): operational guidance and edge-case handling.

## Routing

- Need exact structure or validation rules: read [spec.md](./spec.md).
- Need authoring guidance or policy decisions: read [faq.md](./faq.md).

## Minimal Output Template

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## SemVer Mapping

- `fix` -> `PATCH`
- `feat` -> `MINOR`
- Any commit with `BREAKING CHANGE:` footer or `!` marker -> `MAJOR`

## Core Constraints

> **[CONSTRAINTS]**
> - `BREAKING CHANGE` must be uppercase when used as the special footer token.
> - `BREAKING-CHANGE` is a valid synonym only as a footer token.
> - Body starts after one blank line following the description.
> - Footer section starts after one blank line following the body, or after the description if no body exists.
> - Parsing should be case-insensitive except for the special token `BREAKING CHANGE`.