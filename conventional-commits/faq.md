# Conventional Commits FAQ

## Scope

- Operational guidance distilled from the official FAQ section.
- Intended for agents that need policy answers, authoring recommendations, or fallback behavior.

### [Concept] Initial Development Phase

- Use Conventional Commits even before the first release.
- Treat the project as if it already has users.
- Reason: consumers still need to understand fixes, features, and breaking changes.

### [Concept] Type Casing

- Any casing may be used by the specification.
- Best practice: choose one style and keep it consistent.
- Common convention: lowercase types.

### [Concept] Commit Matches Multiple Types

- Preferred action: split the work into multiple commits whenever possible.
- Rationale: structured commits improve changelogs, review quality, and release automation.

### [Concept] Does This Slow Down Iteration

- It discourages disorganized commits, not fast delivery.
- It supports long-term speed by keeping history machine-readable and reviewable.

### [Concept] Does It Limit Allowed Commit Shapes

- No fixed global list beyond the core conventions.
- Teams may define additional types as needed.
- The spec encourages clearer categorization, not reduced flexibility.

### [Concept] Relation To SemVer

- `fix` -> `PATCH`
- `feat` -> `MINOR`
- Any commit with a breaking-change indicator -> `MAJOR`

### [Concept] Versioning Extensions To The Spec

- Recommended approach: use SemVer for extensions built on top of Conventional Commits.

### [Concept] Wrong Type Used But Still Valid

Example: `fix` used where `feat` was intended.

- Before merge or release: rewrite history, for example with interactive rebase.
- After release: correction depends on the team's tooling and release process.

### [Concept] Invalid Type Used

Example: `feet` instead of `feat`.

- The world does not break.
- The practical consequence is that spec-based tooling may ignore that commit.

### [Concept] Do All Contributors Need To Follow The Spec

- No.
- In squash-merge workflows, maintainers can normalize commit messages at merge time.
- This keeps the burden low for casual contributors.

### [Concept] Revert Commits

- The specification does not fully define revert semantics.
- Tool authors are expected to decide how reverts affect release logic.
- A common pattern is to use the `revert` type and reference reverted SHAs in footers.

```text
revert: let us never again speak of the noodle incident

Refs: 676104e, a215868
```

> **[CONSTRAINTS]**
> - Do not assume the spec itself defines how reverts change version bump calculations.
> - Revert handling should be implemented as project or tool policy.

## Agent Decision Defaults

- If unsure between multiple types, prefer splitting the change instead of inventing a compound type.
- If contributors do not follow the spec, normalize messages at squash or merge boundaries.
- If a commit type is misspelled, do not reinterpret it automatically unless project policy allows correction.
- If revert behavior matters for releases, defer to repository-specific tooling rules.

## Related

- [spec.md](./spec.md)
- [_INDEX.md](./_INDEX.md)