# Quatico Commit Notation

A commit notation that conveys **risk level** and **intention** in the first few characters of each commit message.

## Quick Reference

### Risk Levels

| Risk Level | Code | Example | Meaning |
|------------|------|---------|---------|
| Known safe | lowercase | `r` | Provable correctness (tool-verified, type-checked) |
| Validated | UPPERCASE | `R` | Test-verified, all known risks addressed |
| Risky | `!!` | `R!!` | Attempted but not fully verified |
| Broken | `**` | `R**` | WIP, may not compile |

### Intentions

| Code | Name | Use |
|------|------|-----|
| F | Feature | Add, change, or extend behavior |
| B | Bugfix | Repair undesirable behavior |
| R | Refactoring | Change implementation, not behavior |
| D | Documentation | Changes that communicate, no behavior impact |
| T | Test-only | Alter tests without altering functionality |
| E | Environment | Dev setup, tooling, non-code changes |
| A | Automated | Tool-assisted: IDE refactoring, formatters, linters (deterministic tools only, no AI/LLMs) |
| C | Comment | Comment-only changes (not JSDoc/JavaDoc) |
| S | Spec | Formal specification, design docs |
| * | Unknown | Mixed changes, just checking in |

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

## Intentions

### Feature (F)

**Known Risks**
- May alter unrelated feature (spooky action at a distance)
- May alter a piece of this feature that should remain unchanged
- May implement the change differently than intended

| Code | Approach |
|------|----------|
| `f` | Examples: Local/focused change validated in DEV (no env-specific behavior); Extensive E2E tests; Text-only changes (i18n) apparent from diff |
| `F` | Change ≤8 LoC, fully unit tested, includes new/changed tests |
| `F!!` | Includes unit tests for new behavior |
| `F**` | No automatic tests, or unfinished implementation |

### Bugfix (B)

**Known Risks**
- Customers may depend on the bug
- May alter unrelated feature
- May implement the fix differently than intended

| Code | Approach |
|------|----------|
| `b` | Examples: Local/focused change validated in DEV; E2E tests; Text-only changes; Extend/fix existing test |
| `B` | Reviewed with customer rep, ≤8 LoC, original behavior captured in test, includes changed test |
| `B!!` | Includes unit tests for new behavior |
| `B**` | No automatic tests, or unfinished implementation |

### Refactoring (R)

**Known Risks**
- May cause a bug
- May fix a bug accidentally
- May force a test update

| Code | Approach |
|------|----------|
| `r` | Provable refactoring OR test-supported procedural refactoring within test code |
| `R` | Test-supported procedural refactoring |
| `R!!` | Named refactoring, but edited code without full test coverage |
| `R**` | Remodeled by editing code |

### Documentation (D)

**Known Risks**
- May mislead future developers
- May alter team processes unexpectedly

| Code | Approach |
|------|----------|
| `d` | Dev-visible docs, not in source file, or verified byte-identical compilation |
| `D` | Dev-impacting only, changes compilation or process |
| `D!!` | Alters an important process |
| `D**` | Trying out a process change for info gathering |

### Test-only (T)

| Code | Approach |
|------|----------|
| `t` | Refactoring purely within test code |
| `T` | New test for existing behavior, all tests pass |
| `T!!` | New test without running full suite |
| `T**` | Incomplete test, WIP |

**Alternatives**: Use `F` or `B` depending on what the test validates. Use `R` for refactoring within test code.

### Environment (E)

| Code | Approach |
|------|----------|
| `e` | Config change with no compilation impact |
| `E` | Tooling change, verified working |
| `E!!` | CI/CD change, not fully tested |
| `E**` | Experimenting with build config |

### Automated (A)

Tool-assisted changes where the method (automated tooling) is more significant than the underlying intention.

**When to use A vs R:**
- Use `A` when the tool execution is the primary activity
- Use `R` when performing a specific, named refactoring that happens to use a tool
- Use `A` when changes span multiple intentions or the tool made decisions

**Known Risks**
- Tool may have bugs
- Search & replace may match unintended patterns
- Bulk operations may be hard to review

| Code | Approach |
|------|----------|
| `a` | IDE rename/extract with type verification, compiler confirms no errors |
| `A` | Tool-assisted with test verification |
| `A!!` | Bulk operation (formatter, linter) manually reviewed |
| `A**` | Unverified bulk search/replace |

**Examples**
- `a Rename getUserData to fetchUserProfile` (IDE rename with TypeScript)
- `A: Extract interface IUserService` (IDE extract, tests pass)
- `A!! Format all files with prettier` (bulk format, reviewed)
- `A** Apply regex replace across 20 files` (not fully verified)

### Comment (C)

| Code | Approach |
|------|----------|
| `c` | Comment change, verified byte-identical compilation |
| `C` | Comment change in source file |
| `C!!` | Large comment overhaul |
| `C**` | Draft comments, needs review |

**Alternative**: Use `D`.

### Spec (S)

| Code | Approach |
|------|----------|
| `s` | Spec update matching implemented behavior |
| `S` | Spec change, implementation follows |
| `S!!` | Spec change, implementation pending |
| `S**` | Draft spec for discussion |

**Alternatives**: Use `D`, keep specs outside source control, or use test suite as spec.

### Unknown (*)

Multiple unrelated changes, just getting it checked in. Usually `***`.

**Alternative**: Require each commit to have exactly one intention.

## Appendix

This notation is based on Arlo's Commit Notation v1.

**History:**
- Core intentions (F, B, R, D) from Arlo's original notation
- Extensions (T, E, A, C, S) added by Quatico
