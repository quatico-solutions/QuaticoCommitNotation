# Quatico Commit Notation

A commit notation that conveys **risk level** and **intention** in the first few characters of each commit message.

## Quick Reference

| Risk Level | Code | Example | Meaning |
|------------|------|---------|---------|
| Known safe | lowercase | `r` | Provable correctness (tool-verified, type-checked) |
| Validated | UPPERCASE | `R` | Test-verified, all known risks addressed |
| Risky | `XX!!` | `R!!` | Attempted but not fully verified |
| Broken | `XX**` | `R**` | WIP, may not compile |

## Intentions

### Core

| Code | Name | Use |
|------|------|-----|
| F | Feature | Add, change, or extend behavior |
| B | Bugfix | Repair undesirable behavior |
| R | Refactoring | Change implementation, not behavior |
| D | Documentation | Changes that communicate, no behavior impact |

### Extensions

| Code | Name | Use |
|------|------|-----|
| T | Test-only | Alter tests without altering functionality |
| E | Environment | Dev setup, tooling, non-code changes |
| A | Automated | Tool-assisted: IDE refactoring, formatters, linters (deterministic tools only, no AI/LLMs) |
| C | Comment | Comment-only changes (not JSDoc/JavaDoc) |
| S | Spec | Formal specification, design docs |
| * | Unknown | Mixed changes, just checking in |

See [Extension Intentions.md](Extension%20Intentions.md) for detailed guidance on each extension.

## Commit Message Format

```
X: Summary in active voice

TICKET-123
```

**Rules:**
- **Prefix**: Risk level + intention (e.g., `F`, `r`, `B!!`, `R**`)
- **Separator**: `:` or nothing (e.g., `F: Add feature` or `F Add feature`)
- **Voice**: Active ("Add feature") not progressive ("Adding feature")
- **Length**: Subject line < 75 characters
- **Ticket**: Last line, blank line before it, NO `#` prefix (git treats `#` as comment)

### Examples

```
F: Add user authentication endpoint

FOO-123
```

```
r Extract method calculateTotal

BAR-456
```

```
B!! Fix race condition in event handler

BAZ-789
```

```
A!! Format all files with prettier
```

### When to Omit Ticket

Infrastructure changes may omit tickets:
- CI/CD fixes
- Dev environment setup
- Release version bumps
- Dependency updates
- Code formatting

### Finding the Ticket

1. **Branch name**: Look for `XXX-123` pattern (e.g., `feature/FOO-123-login`)
2. **Recent commits**: Check commits not yet merged to main branch
3. **Ask**: Is this infrastructure (omit) or does it need a ticket number?

## Risk Level Guidelines

### Known Safe (lowercase)

Provable correctness through:
- IDE refactoring with type verification
- Compiler-verified transformations
- Automated tool with known behavior

### Validated (UPPERCASE)

Test-verified changes:
- All tests pass after change
- New behavior covered by tests
- For features/bugfixes: change is ≤8 LoC

### Risky (`!!`)

Attempted verification but gaps remain:
- Tests exist but coverage incomplete
- Manual review performed
- Named refactoring without full test suite

### Broken (`**`)

No verification:
- WIP checkpoint
- May not compile
- Switching tasks, need to save state

## Appendix

This notation is based on Arlo's Commit Notation v1 with Quatico extensions (T, E, A, C, S).
