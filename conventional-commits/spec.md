# Conventional Commits 1.0.0

## Scope

- Lightweight convention for Git commit messages.
- Designed for both human readability and machine parsing.
- Intended to support changelog generation, release automation, and SemVer inference.

### [Concept] Canonical Structure

```text
<type>[optional scope][optional !]: <description>

[optional body]

[optional footer(s)]
```

- `type`: required noun-like token such as `feat`, `fix`, `docs`.
- `scope`: optional noun in parentheses, describing the affected subsystem.
- `!`: optional breaking-change marker placed immediately before `:`.
- `description`: required short summary.
- `body`: optional free-form detail.
- `footer(s)`: optional trailer-style metadata.

### [Concept] SemVer Interpretation

- `fix` implies a bug fix and correlates with `PATCH`.
- `feat` implies a new feature and correlates with `MINOR`.
- `BREAKING CHANGE` or `!` implies incompatible change and correlates with `MAJOR`.
- Non-core types have no implicit SemVer meaning unless they also indicate a breaking change.

### [Concept] Header Grammar

```text
<type>(<scope>)!: <description>
<type>(<scope>): <description>
<type>!: <description>
<type>: <description>
```

- Required sequence: `type` -> optional `scope` -> optional `!` -> `: ` -> `description`.
- The colon must be followed by a single space before the description.
- Scope, when present, must be enclosed in parentheses.
- Description must immediately follow the `: ` separator.

### [Concept] Core Types

- `feat`: add a new feature.
- `fix`: patch a bug.
- Other allowed examples: `build`, `chore`, `ci`, `docs`, `perf`, `refactor`, `style`, `test`, `revert`.
- The specification permits custom types.

### [Concept] Scope

- Optional.
- Must be a noun describing a section of the codebase.
- Format: `type(scope): description`
- Example: `fix(parser): handle escaped commas`

### [Concept] Description

- Required.
- Short summary of the change.
- Should describe the actual modification, not workflow metadata.
- Example: `fix: array parsing issue when multiple spaces are contained in string`

### [Concept] Body

- Optional.
- Begins one blank line after the header.
- Free-form text.
- May contain multiple paragraphs separated by newlines.
- Used for rationale, context, implementation notes, or side effects.

### [Concept] Footer Format

- Optional.
- Begins one blank line after the body.
- Each footer consists of:
  - a token
  - either `: ` or ` #` as separator
  - a string value
- Inspired by Git trailer format.

Accepted patterns:

```text
Reviewed-by: Alice
Refs: #123
Closes: #456
```

### [Concept] Footer Token Rules

- Footer tokens normally use `-` instead of spaces.
- Example: `Acked-by`
- Special exception: `BREAKING CHANGE`
- `BREAKING-CHANGE` is synonymous with `BREAKING CHANGE` when used as a footer token.
- Footer values may span spaces and newlines.
- Footer parsing ends when the next valid token/separator pair begins.

### [Concept] Breaking Change Signaling

Two valid mechanisms:

1. Header marker:

```text
feat!: remove legacy auth flow
feat(api)!: drop v1 endpoints
```

2. Footer token:

```text
BREAKING CHANGE: environment variables now take precedence over config files
```

Rules:

- At least one of the two mechanisms must be present to mark a breaking change.
- If `!` is used, the `BREAKING CHANGE:` footer may be omitted.
- If only `!` is used, the description should explain the breaking change.

> **[CONSTRAINTS]**
> - `!` must appear immediately before `:`.
> - `BREAKING CHANGE` must be uppercase when used as the special footer token.
> - A breaking change can appear on any commit type, not only `feat` or `fix`.

### [Concept] Normative Rules

1. Commit messages must start with a type.
2. Type is followed by optional scope, optional `!`, then required `: `.
3. `feat` must be used for new features.
4. `fix` must be used for bug fixes.
5. Scope is optional but, when present, must be a noun inside parentheses.
6. Description is required and immediately follows `: `.
7. Body is optional and starts after one blank line.
8. Body may contain any number of paragraphs.
9. One or more footers may appear after one blank line.
10. Footer syntax is `token: value` or `token #value`.
11. Footer tokens use hyphens instead of spaces, except `BREAKING CHANGE`.
12. Footer values may span multiple lines.
13. Breaking changes must be indicated by header `!` or footer token.
14. Types beyond `feat` and `fix` are allowed.
15. Parsing should not treat units as case-sensitive, except `BREAKING CHANGE` which must remain uppercase.
16. `BREAKING-CHANGE` and `BREAKING CHANGE` are equivalent footer tokens.

### [Concept] Parser Guidance

- Treat the commit as three logical regions: header, body, footer.
- Identify the header from the first line.
- Split body/footer only on blank-line boundaries.
- Detect footer candidates by valid token plus separator.
- Do not infer SemVer bump from custom types unless project policy defines one.
- Always prioritize explicit breaking-change markers over type-based inference.

### [Concept] Valid Examples

Feature with breaking footer:

```text
feat: allow provided config object to extend other configs

BREAKING CHANGE: extends key in config file is now used for extending other config files
```

Breaking change via header marker:

```text
feat!: send an email to the customer when a product is shipped
```

Breaking change with scope:

```text
feat(api)!: send an email to the customer when a product is shipped
```

Non-breaking documentation change:

```text
docs: correct spelling of CHANGELOG
```

Scoped feature:

```text
feat(lang): add Polish language
```

Body plus multiple footers:

```text
fix: prevent racing of requests

Introduce a request id and a reference to latest request. Dismiss
incoming responses other than from latest request.

Remove timeouts which were used to mitigate the racing issue but are
obsolete now.

Reviewed-by: Z
Refs: #123
```

Revert-style example:

```text
revert: let us never again speak of the noodle incident

Refs: 676104e, a215868
```

### [Concept] High-Risk Invalid Patterns

- Missing colon separator: `feat add login`
- Missing space after colon: `fix:missing space`
- Invalid breaking marker placement: `feat(!): change api`
- Lowercase special footer token when strict compatibility is required: `Breaking Change: ...`
- Scope without parentheses: `fix parser: ...`
- Body or footer placed without required blank-line separation.

## Agent Output Heuristics

- For release classification:
  - if `BREAKING CHANGE` footer exists or header contains `!` -> `MAJOR`
  - else if type is `feat` -> `MINOR`
  - else if type is `fix` -> `PATCH`
  - else -> no implied bump
- For commit generation:
  - choose one primary type per commit
  - split unrelated changes into separate commits when possible
  - include body only when additional context is useful
  - include footers for issue references, reviews, and breaking details

## Related

- [faq.md](./faq.md)
- [_INDEX.md](./_INDEX.md)