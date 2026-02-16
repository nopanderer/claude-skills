# Conventional Commits Reference Guide

## Summary

Conventional Commits is "a lightweight convention on top of commit messages" that establishes structured rules for creating commit histories. This enables automated tooling and aligns with Semantic Versioning by documenting features, fixes, and breaking changes.

## Commit Message Structure

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Commit Types

The specification defines these structural elements:

- **fix**: Patches a bug (maps to PATCH in SemVer)
- **feat**: Introduces a new feature (maps to MINOR in SemVer)
- **BREAKING CHANGE**: Indicates breaking API changes using either a footer or `!` after type/scope (maps to MAJOR in SemVer)
- **Other types**: build, chore, ci, docs, style, refactor, perf, test (allowed but not mandated)

## Key Format Rules

- Type is required; scope is optional and appears in parentheses
- Breaking changes indicated by `!` before the colon or via footer
- Description follows the colon immediately
- Body begins one blank line after description
- Footers use dash-separated tokens with `: ` or ` #` separators

## Examples

| Use Case | Format |
|----------|--------|
| Breaking change with footer | `feat: allow config extension`<br><br>`BREAKING CHANGE: extends key behavior changed` |
| Breaking change with `!` | `feat!: send product shipment email` |
| With scope and `!` | `feat(api)!: send product shipment email` |
| No body | `docs: correct CHANGELOG spelling` |
| With scope | `feat(lang): add Polish language` |

## Benefits

- Generates automated CHANGELOGs
- Determines semantic version bumps automatically
- Communicates changes to stakeholders
- Triggers build and publish processes
- Structures commit history for easier project contribution

---

**License**: Creative Commons - CC BY 3.0
